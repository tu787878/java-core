# Motivation

In production filesystem can corrupt, networks break down, and JVM can run out of memory. 
The wellbeing of our code depends on how it deals with "unhappy path".

> Problem:

Without handling exceptions, an otherwise healty program may stop running altogether.

### finally

When we have the code that need to execute regardless of whether an exception occurs, and this is where the finally keywork comes in.

### checked exception vs unchecked exception???

> checked exception:

Checked exceptions must be explicitly caught or propagated it in the inheritance hierarchy.

Any Exception that can be thrown by a method is part of the method's public programming interface. Those who call a method must know about the exceptions that a method can throw so that they can decide what to do about them

> unchecked exception:

Runtime exceptions represent problems that are the result of a programming problem, and as such, the API client code cannot reasonably be expected to recover from them or to handle them in any way.

Unchecked exceptions do not have this requirement. They don't have to be caught or declared thrown

The runtime exceptions are generally intended to be not caught at all, or only caught at the bottom of the call stack, in the UI layer, in order to display an error message, because that's usually the only thing you can do when such an exception occurs.

If the client can't reasonably be expected to recover from the situation, there's no point in forcing it to catch the exception. In such unrecoverable situation an unchecked exception should be used.




>  If a client can reasonably be expected to recover from an exception, make it a checked exception. If a client cannot do anything to recover from the exception, make it an unchecked exception.

## Usage
Use checked exceptions when the caller can reasonably recover from the failure and the failure is outside the program's control — file not found, network timeout, database connection lost. You're telling the compiler "this can go wrong, force the caller to deal with it."
Use unchecked exceptions when the problem is a programming error — null where it shouldn't be, invalid argument, index out of bounds. These represent bugs, not recoverable situations. The caller shouldn't be expected to catch them; the code should be fixed instead.

# Summary
- Checked → caller can recover, external system failures (IOException, SQLException)
- Unchecked → programming bugs, invalid state, precondition violations (IllegalArgumentException, NullPointerException, IllegalStateException)
- Error → JVM-level, never catch (OutOfMemoryError, StackOverflowError)


it's a genuine trade-off, not a clear winner -> It depends on context.