# 1. Consumer
We use consumers when we need to consume objects.

`void accept(T value);`

`default Consumer<T> andThen(Consumer<? super T> after);`


```
Consumer<String> printConsumer= city-> System.out.println(city);    
    cities.forEach(printConsumer);
```

# 2. Predicate
A ***Predicate*** is a functional interface that accepts an argument and return a Boolean. Usually, it is used to apply a filter for a collection of objects.

```
boolean test(T value);
default Predicate<T> and(Predicate<? super T> other);
default Predicate<T> negate();
default Predicate<T> or(Predicate<? super T> other);
static <T> Predicate<T> isEqual(Object targetRef);
static <T> Predicate<T> not(Predicate<? super T> target);
```

Examples:
```
Predicate<String> filterCity = city -> city.equals("Mumbai");
cities.stream().filter(filterCity).forEach(System.out::println);
```

# 3. Function
The function takes an input value and returns a value.
```
R apply(T var1);
default <V> Function<V, R> compose(Function<V, T> before);
default <V> Function<T, V> andThen(Function<R, V> after);
static <T> Function<T, T> identity();
```
Examples:
```
Function<String, Character> getFirstCharFunction = city -> {
        return city.charAt(0);
    };
cities.stream().map(getFirstCharFunction)
                   .forEach(System.out::println);
```

# 4. Supplier
The Supplier does not take any argument but produces a value of type T.

`T get();`

Exampls:

```
Supplier<String[]> citySupplier = () -> {
        return new String[]{"Mumbai", "Delhi", "Goa", "Pune"};
    };
Arrays.asList(citySupplier.get()).forEach(System.out::println);
```


# What is a lambda expression?
You want to define a anoynymous function.

Lambda expression is an short, anonymous function that provides a direct implementation of a functional interface without the boilerplate of an anonymous inner class.

# What is a functional interface?
A functional interface is an interface that has only one abstract method.

# Optional
Help us to explicit handle potentially nullable values without causing ***NullPointerExceptions***.