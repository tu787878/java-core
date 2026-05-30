# 1. The immutable of String in java
In Java ***String*** is immutable, meaning once the object String is created, its value cannot be changed.

```
String greeting = "Hello";
greeting = greeting + " World";  // A new String object is created
System.out.println(greeting);  // Output: Hello World
```
In this example the concatenating of " World" does not modify the original string but rather creates a new one.

> Java has an internal String Pool to manage String objects efficiently. (Garbage collection)

```
private String alphabetConcat() {
    StringBuilder series = new StringBuilder();
    for (int i = 0; i < 26; i++) {
        series.append((char) ('a' + i));
        System.out.println(series); // Outputs: a ab abc abcd ...
    }
    return series.toString();
}
```
In this case use ***StringBuilder*** and ***StringBuffer*** rather than String because of the mutablility

More memory efficiency, descreasing garbage collection overhead by prevent creating new object of string every concatenant operation.