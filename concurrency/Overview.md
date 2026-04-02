#1. What is concurrency???
- ***Multiple threads*** of program executing concurrently
- We must understand which ***resources*** threads share why executing program to avoid ***unexpected results***? 
- A code is ***thread-safe*** if it behaves correctly when accessed from multiple threads.

---
## Recall
> ### Memory
The JVM devides the memory into two parts: *stack memory* and *heap memory*.
The *variables* allocated in *stack memory* and are confined to a single thread.
*Objects*, *Class members*, and *Static* are stored in the *heap memory* and all threads can access them.


## Solution

1. Use ***volatile*** to ensure that only one thread accesses the variable at a time
`private static volatile int count=0;`

> Becarefully, the ***volatile*** variables can only guarantee *visibility* not *atomicity*.
>> What are non-atomic operations: e.g. fetch the current value, add one, and write the new value back,  *count++*, *count--*, ...

2. Use ***synchronize***:
`public synchronized void buy()
{
        count--;
        }`

#2.  Thread-Based Concurrency Model 
`Thread thread = new Thread(() -> {
            System.out.println("Hello from thread!");
        });
        thread.start();`
        

#3.  Executor-Based Concurrency Model 
`ExecutorService executor = Executors.newFixedThreadPool(5);
        executor.execute(() -> {
            System.out.println("Hello from executor!");
        });`
        
        
#4.   Parallel Stream Concurrency Model 
`Stream.stream(java.util.Arrays.asList(1, 2, 3, 4, 5)).parallel().forEach(System.out::println);`


