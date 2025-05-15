## 📝 Assignment: **Logging / Audit Trail Writer (Simulation)**

### 🎯 **Problem Background**

In real-world applications — especially in **banking**, **healthcare**, or **e-commerce** — it’s essential to keep an **audit trail** of critical actions. Each time a user performs an action (like transferring money or accessing sensitive data), we need to log that event asynchronously, so we don’t slow down the main system flow.

> ❗ Logs must be recorded in real-time but should **not block** the main user operations.

We’ll simulate an **audit logging system** using Java threads. Each "user action" will trigger a log entry, and we’ll handle the logging asynchronously using threads.

---

## 🧠 **Algorithmic Thinking & Design Discussion**

Help students break it down:

| Task              | What to Think About                                                    |
| ----------------- | ---------------------------------------------------------------------- |
| Action simulation | How do we simulate a user event like "Login" or "Transfer"?            |
| Logging           | How can we log messages in real-time without blocking the main thread? |
| Thread use        | Should logging be in the same thread? (No – it must be separate.)      |
| Logging queue     | What happens if multiple actions are triggered at once?                |
| Output            | Where do we log? For this simulation: print to console.                |

---

## 🔄 Steps to Follow

1. **Create an `AuditLogger` class** that implements `Runnable`.
2. Accept a log message and simulate writing the log (with `Thread.sleep()`).
3. In the main app, simulate multiple user actions.
4. For each action, create a new thread for logging.
5. Use `join()` so main thread waits for logs to complete (optional for realism).
6. Document the theory with `JavaDoc`.

---

## 💻 Starter Code with TODOs

### 🔧 `AuditLogger.java`

```java
// TODO: Declare the package 'com.example.audit'

/**
 * The AuditLogger class is a task that simulates writing a log entry
 * asynchronously. It implements Runnable so it can be executed by a Thread.
 *
 * Threads are ideal here because we want to simulate concurrent logging 
 * without blocking the main execution flow.
 */

// TODO: Define a public class named 'AuditLogger' that implements the 'Runnable' interface

// TODO: Declare a private final field 'message' of type String to store the log message
 /**
     * Constructs an AuditLogger with the log message.
     *
     * @param message The audit message to be logged.
     */
// TODO: Create a constructor that accepts a String message and assigns it to the message field

 /**
     * The run method contains the logic for logging.
     * We simulate a delay using Thread.sleep() to mimic real-world logging latency.
     */
// TODO: Override the 'run' method from Runnable interface. This is where logging logic will go.

    // TODO: Inside 'run', print a message that logging has started. Include the current thread's name and the log message.

    // TODO: Use Thread.sleep() to simulate a delay in logging (1000 milliseconds)

    // TODO: Catch the InterruptedException that might occur during sleep
        // TODO: Print an error message indicating the logging was interrupted
        // TODO: Reset the thread's interrupted status using Thread.currentThread().interrupt()

    // TODO: After the delay, print a message that logging has finished. Again, include the thread name and the message.

```

---

### 🚀 `AuditSimulationApp.java`

```java
// TODO: Declare the package 'com.example.audit'

// TODO: Import required classes from java.util for working with Lists and Arrays

/**
 * Main class to simulate audit trail logging using threads.
 *
 * Theory:
 * In real systems, audit logs are often handled by a logging framework
 * or background thread to avoid blocking user-facing processes.
 */
// TODO: Define a public class named 'AuditSimulationApp' — this will be the main entry point

    // TODO: Define the main method — this is where the simulation starts

        // TODO: Create a List of Strings to represent simulated user actions or audit log messages

        // TODO: Loop through each action in the list
            // TODO: For each action, create an instance of AuditLogger with the action as the message
            // TODO: Create a new Thread using the AuditLogger instance and give it a unique name using hashCode
            // TODO: Start the thread to simulate asynchronous logging

            // OPTIONAL TODO: Use join() to wait for the thread to finish (uncomment block to enable)
                // TODO: Wrap join in try-catch for InterruptedException
                // TODO: If interrupted, print error and reset thread's interrupt flag

        // TODO: After the loop, print a message indicating that all audit log threads have been started

```

---

## 📚 Java Concepts Used

| Concept        | Why It’s Used                                |
| -------------- | -------------------------------------------- |
| `Runnable`     | Clean way to define logging as a task        |
| `Thread`       | Executes each logging task asynchronously    |
| `sleep()`      | Simulates I/O delay during logging           |
| `join()`       | Optional use to wait for logging threads     |
| `List<String>` | Simulates user actions triggering log events |

---

## 🔁 Extension Tasks with JUnit 5 with Starter Code (Optional for Curious Minds)
```java
package com.example.audit;

import org.junit.jupiter.api.*;
import org.junit.jupiter.params.ParameterizedTest;
import org.junit.jupiter.params.provider.ValueSource;

import java.io.ByteArrayOutputStream;
import java.io.PrintStream;
import java.util.*;

/**
 * 📘 This test class contains various tasks designed to help students test
 * the `AuditLogger` class, which simulates asynchronous audit logging using threads.
 *
 * 🧪 Students will:
 * - Capture and verify console output
 * - Launch and join multiple threads
 * - Use @Timeout to check execution limits
 * - Interrupt a thread and test graceful shutdown
 * - Use @ParameterizedTest to run the same logic for multiple inputs
 *
 * 🚀 These tests aim to deepen understanding of concurrency, exception handling,
 * and JUnit 5 capabilities in a real-world scenario.
 */
public class AuditLoggerTest {

    /**
     * ✅ Task 1: Verify that the logger prints the correct start and end messages.
     * This uses a trick called *output redirection* — we temporarily reroute System.out
     * so we can capture and inspect what's printed.
     */
    @Test
    void shouldLogCorrectMessage() {
        // Redirect System.out to capture output in a string stream
        ByteArrayOutputStream outContent = new ByteArrayOutputStream();
        System.setOut(new PrintStream(outContent));

        String message = "Test log entry";
        AuditLogger logger = new AuditLogger(message);

        // We call run() directly here instead of using a thread
        logger.run();

        // Reset System.out back to default
        System.setOut(System.out);

        // Check that expected content appears in output
        String output = outContent.toString();
        Assertions.assertTrue(output.contains("Logging STARTED"));
        Assertions.assertTrue(output.contains("Logging FINISHED"));
        Assertions.assertTrue(output.contains(message));
    }

    /**
     * ✅ Task 2: Start multiple threads to simulate concurrent logging.
     * We want to test that all threads finish successfully without errors.
     */
    @Test
    void shouldExecuteAllThreads() throws InterruptedException {
        List<String> messages = Arrays.asList("A", "B", "C");
        List<Thread> threads = new ArrayList<>();

        // Create and start a thread for each log message
        for (String msg : messages) {
            Thread t = new Thread(new AuditLogger(msg));
            threads.add(t);
            t.start(); // Launch the thread
        }

        // Wait for all threads to finish using join()
        for (Thread t : threads) {
            t.join(); // join() blocks until the thread finishes
        }

        // If all threads completed without exception, the test passes
    }

    /**
     * ✅ Task 3: Ensure logging completes within a time limit.
     * If it takes longer than 2 seconds, the test fails automatically.
     */
    @Timeout(2) // Max time allowed for this test
    @Test
    void shouldFinishWithinTimeLimit() {
        AuditLogger logger = new AuditLogger("Speed test");
        logger.run(); // run() includes Thread.sleep(1000), so 2s is safe
    }

    /**
     * ✅ Task 4: Test how the logger handles interruption.
     * We start the logger in a thread, interrupt it, and check that it exits gracefully.
     */
    @Test
    void shouldHandleInterruption() throws InterruptedException {
        Thread thread = new Thread(new AuditLogger("Interrupt test"));
        thread.start();

        // Ask the thread to stop sleeping by interrupting it
        thread.interrupt();

        // Wait for thread to wrap up
        thread.join();

        // Check if the thread was interrupted or completed
        Assertions.assertTrue(thread.isInterrupted() || !thread.isAlive());
    }

    /**
     * ✅ Task 5: Test the logger using multiple inputs automatically.
     * This avoids copy-pasting the same test many times.
     */
    @ParameterizedTest
    @ValueSource(strings = {
            "Login",
            "Transfer",
            "Logout"
    })
    void shouldLogAllMessages(String message) {
        // Redirect output to inspect the log result
        ByteArrayOutputStream outContent = new ByteArrayOutputStream();
        System.setOut(new PrintStream(outContent));

        AuditLogger logger = new AuditLogger(message);
        logger.run();

        // Reset System.out
        System.setOut(System.out);

        // Verify the message was printed
        String output = outContent.toString();
        Assertions.assertTrue(output.contains(message));
    }

    /**
     * 🧠 Reference - Concepts Covered:
     * - JUnit 5 basics: @Test, @BeforeEach, Assertions.*
     * - JUnit 5 advanced: @Timeout, @ParameterizedTest, @ValueSource
     * - Capturing console output using ByteArrayOutputStream
     *
     * 📚 Suggested Learning Resources:
     * - https://junit.org/junit5/docs/current/user-guide/
     * - https://www.baeldung.com/junit-5
     */
}


```


---

## ✅ Submission Instructions

1. Complete the TODOs in both files
2. Ensure logs show start and finish clearly, with thread names
3. Push your working code to GitHub and share the link


