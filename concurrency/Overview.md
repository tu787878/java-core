# 1. What is concurrency???
- ***Multiple threads*** of program executing concurrently
- We must understand which ***resources*** threads share why executing program to avoid ***unexpected results***? 
- A code is ***thread-safe*** if it behaves correctly when accessed from multiple threads.

- Threads can share the same memory and resources. The shared state makes concurrency possible but also introduces risks, requiring careful synchronnization.

---
## Recall
> ### Memory
The JVM devides the memory into two parts: *stack memory* and *heap memory*.
The *variables* allocated in *stack memory* and are confined to a single thread.
*Objects*, *Class members*, and *Static* are stored in the *heap memory* and all threads can access them.

> ### Cooperative vs Preemptive Multitasking
- ***Cooperative***: The program decides when to give up CPU. ( Early Windows/Mac)
- ***Preemptive***: OS decides. (Modern OS) 

> ### Concurrency  vs Parallelism
-  Key difference → ***Concurrency*** = dealing with many things. ***Parallelism*** = doing many things.
## Solution

### 1. Use ***volatile***: 
to ensure that only one thread accesses the variable at a time
`private static volatile int count=0;`

> Becarefully, the ***volatile*** variables can only guarantee *visibility* not *atomicity*.
>> What are non-atomic operations: e.g. fetch the current value, add one, and write the new value back,  *count++*, *count--*, ...

### 2. Use ***synchronized***:
`public synchronized void buy()
{
        count--;
        }`

### 3.  ***Thread-Based*** Concurrency Model:
`Thread thread = new Thread(() -> {
            System.out.println("Hello from thread!");
        });
        thread.start();`
        

### 4.  ***Executor-Based*** Concurrency Model:
`ExecutorService executor = Executors.newFixedThreadPool(5);
        executor.execute(() -> {
            System.out.println("Hello from executor!");
        });`
        
        
### 5.   ***Parallel Stream*** Concurrency Model:

`Stream.stream(java.util.Arrays.asList(1, 2, 3, 4, 5)).parallel().forEach(System.out::println);`

### 6. Concurrent Collections:
- ConcurrentHashMap
- CopyOnWriteArrayList 

### 7. CompletableFuture and Reactive Streams:
CompletableFuture provides a way to write non-blocking, asynchronous code in Java.

- Creating and Combining CompletableFutures
```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "Hello")
            .thenApplyAsync(greeting -> greeting + " World")
            .thenApplyAsync(message -> message + "!");
        System.out.println(future.get()); // Prints "Hello World!"
```
- Exception Handling in Asynchronous TasksException Handling in Asynchronous Tasks
```java
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            if (true) {
                throw new RuntimeException("Something went wrong!");
            }
            return "Hello";
        }).exceptionally(ex -> "Recovered from: " + ex.getMessage());
        try {
            System.out.println(future.get()); // Prints "Recovered from: Something went wrong!"
        } catch (InterruptedException | ExecutionException e) {
            e.printStackTrace();
        }
```

# Real-Life Examples
## Managing Database Connections
In a web application, a limited number of database connections can be managed using a *semaphore*, ensureing efficient use of resources without  overwhelming the database server.

```java
import java.util.concurrent.Semaphore;
public class DatabaseConnectionPool 
{
    private final Semaphore semaphore;
    public DatabaseConnectionPool(int maxConnections) 
    {
        semaphore = new Semaphore(maxConnections);
    }
    public void connect() 
    {
        try 
        {
            semaphore.acquire();
            // Simulate database connection usage
        } 
        catch (InterruptedException e) 
        {
            Thread.currentThread().interrupt();
        } 
        finally 
        {
            semaphore.release();
        }
    }
}
```
## Web Server Handling Multiple Client Requests
A web server can hanlde multiple client requests conccurently using  a thread pool. 

```java
import java.io.IOException;
import java.net.ServerSocket;
import java.net.Socket;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class WebServer 
{
    private static final int PORT = 8080;
    private static final int THREAD_POOL_SIZE = 10;
    public static void main(String[] args) 
    {
        ExecutorService executor = Executors.newFixedThreadPool(THREAD_POOL_SIZE);
        try (ServerSocket serverSocket = new ServerSocket(PORT)) {
            while (true) 
            {
                Socket clientSocket = serverSocket.accept();
                executor.execute(new ClientHandler(clientSocket));
            }
        } 
        catch (IOException e) 
        {
            e.printStackTrace();
        }
    }
}
class ClientHandler implements Runnable 
{
    private final Socket clientSocket;
    public ClientHandler(Socket clientSocket) 
    {
        this.clientSocket = clientSocket;
    }
    @Override
    public void run() 
    {
        // Handle client request
    }
}

```

## Concurrent Caching Mechanism

```java
import java.util.concurrent.ConcurrentHashMap;

public class Cache<K, V> 
{
    private final ConcurrentHashMap<K, V> map = new ConcurrentHashMap<>();
    public V get(K key) 
    {
        return map.get(key);
    }
    public void put(K key, V value) 
    {
        map.put(key, value);
    }
    public void remove(K key) 
    {
        map.remove(key);
    }
}
```

## Non-blocking I/O Operations in a Web Application
```java
import java.util.concurrent.CompletableFuture;

public class NonBlockingIOExample 
{
    public static void main(String[] args) 
    {
        CompletableFuture.supplyAsync(() -> performIO())
            .thenAccept(result -> System.out.println("I/O Result: " + result))
            .exceptionally(ex -> 
            {
                System.err.println("Error: " + ex.getMessage());
                return null;
            });
    }
    private static String performIO() 
    {
        // Simulate I/O operation
        return "I/O Operation Completed";
    }
}
```

## Real-time Data Processing in a Stock Trading Application
Reactive streams can be used to process real-time stock market data, allowing applications to react to price changes and execute trades automatically.

```java

import reactor.core.publisher.Flux;

public class StockTradingExample {
    public static void main(String[] args) 
    {
        Flux.just(100, 102, 101, 103, 105)
            .filter(price -> price > 102)
            .subscribe(price -> System.out.println("Trade executed at price: " + price));
    }
}
```

## File Uploading in the Background
```java
public class FileUploader implements Runnable {
    private String fileName;

    public FileUploader(String fileName) {
        this.fileName = fileName;
    }

    @Override
    public void run() {
        System.out.println("Uploading: " + fileName);
        try { Thread.sleep(2000); } catch (InterruptedException e) { }
        System.out.println("Upload complete: " + fileName);
    }

    public static void main(String[] args) {
        new Thread(new…
```

# Best Practices in Concurrency
## Ensuring Thread Safety
- Immutability (mark ***final***): Make objects immutable to avoid synchronization issues.
- Atomic Variables and Operations: Use atomic variables and operations to ensure thread safety.
```java
        private final AtomicInteger counter = new AtomicInteger(0);
        public void increment() 
        {
        counter.incrementAndGet();
        }
        public int getCounter() 
        {
        return counter.get();
        }
```