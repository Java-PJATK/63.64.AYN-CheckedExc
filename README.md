# AYN-CheckedExc

63  
64  
AYN-CheckedExc/MyUncheckedException.java  
AYN-CheckedExc/CheckedExc.java  
105  
106  

```markdown
### Custom Exceptions and Assertions

#### Custom Exception Example
**Listing 63**
**AYN-CheckedExc/MyUncheckedException.java**
```java
public class MyUncheckedException extends IllegalArgumentException {
    MyUncheckedException() {
        super();
    }
    MyUncheckedException(String message) {
        super(message);
    }
}
```

#### Using Custom Exception
**Listing 64**
**AYN-CheckedExc/CheckedExc.java**
```java
public class CheckedExc {

    public static void main(String[] args) {

        try {
            goSleep(5*1000);
        } catch (InterruptedException ignored) {
            System.err.println("Interrupted");
        } finally {
            System.err.println("AFTER SLEEP");
        }

        // Handling all exceptions
        try {
            goSleep(-1);
        } catch (InterruptedException e) {
            System.err.println("Interrupted");
        } catch (Exception e) {
            System.err.println("Handling exception: " + e.getMessage());
        } finally {
            System.err.println("GOING ON");
        }

        // Here MyUncheckedException is not handled
        // so the program will crash (but 'finally' clause will be executed anyway)
        try {
            goSleep(-1);
        } catch (InterruptedException e) {
            System.err.println("Interrupted");
        } finally {
            System.err.println("QUITTING");
        }
    }

    private static void goSleep(int time) throws InterruptedException {
        if (time < 0)
            throw new MyUncheckedException("Negative time");

        System.out.println("Going to sleep for " + time +"ms");
        Thread.sleep(time);
        System.out.println("Waking up");
    }
}
```

The program behavior:
```
Going to sleep for 3000ms
Waking up
AFTER SLEEP
Handling exception: Negative time
GOING ON
QUITTING
Exception in thread "main" MyUncheckedException: Negative time
at CheckedExc.goSleep(CheckedExc.java:40)
at CheckedExc.main(CheckedExc.java:29)
```

## 11.6 Assertions

Another way of dealing with possible errors in our programs is by using assertions (line 34 of the listing 60 on page 102). After the keyword `assert`, we specify a condition (something with boolean value) and, after a colon, a message (which is in fact optional):

```java
assert boolExp : message;
```

At runtime, the value of `boolExp` is evaluated and if found false, the program will print the `message` and terminate by throwing an exception of type `AssertionError`.

In our example, this is what will happen when the file is missing and the variable `scfile` is null. By default, assertions are disabled at runtime, but can be enabled by `-ea` switch when running a program. Because assertions are sometimes on and sometimes off, there should be no side effects of using them. Normally, assertions check conditions that we are almost sure should always hold. If, however, the condition is not met, and an unchecked exception of type `AssertionError` is thrown, we should never even try to handle it, because it indicates a serious flaw in the program that just must be corrected.
