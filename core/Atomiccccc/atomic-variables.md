# Simple operation: ++, --,... are non-atomic

Example: ++: obtaining the value, incrementing, and writing the updated value back


Using lock (synchronized) solves the problem, but the performance take a hit.
> The process of suspending and then resuming a thread is very expensive.

Some rules:
- All referrence assigments are atomic in java. 
- Object construction is not atomic.
```

String str1 = "foo";                // line 1, atomic
String str2 = "foo" + "bar";        // line 2, atomic
String str3 = str1;                 // line 3, atomic
String str4 = str1 + str2;          // line 4, not atomic
String str5 = new String("foobar"); // line 5, not atomic

```

# What is the difference between String, StringBuilder, and StringBuffer? When would you use each?

- String is immutable
- StringBuilder is mutable for single thread
- StringBuffer is mutable and threadsafe