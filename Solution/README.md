## ✅ **Solution: Logging / Audit Trail Writer (Simulation)**

---

### 🔧 `AuditLogger.java`

```java
package com.example.audit;

/**
 * The AuditLogger class is a task that simulates writing a log entry
 * asynchronously. It implements Runnable so it can be executed by a Thread.
 *
 * Threads are ideal here because we want to simulate concurrent logging 
 * without blocking the main execution flow.
 */
public class AuditLogger implements Runnable {

    // The message to log (immutable)
    private final String message;

    /**
     * Constructs an AuditLogger with the log message.
     *
     * @param message The audit message to be logged.
     */
    public AuditLogger(String message) {
        this.message = message;
    }

    /**
     * The run method contains the logic for logging.
     * We simulate a delay using Thread.sleep() to mimic real-world logging latency.
     */
    @Override
    public void run() {
        // Print that logging has started, including thread name
        System.out.println("Logging STARTED for: " + message + " | by " + Thread.currentThread().getName());

        try {
            // Simulate log writing delay
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            System.err.println("Logging interrupted for: " + message);
            Thread.currentThread().interrupt(); // Reset the interrupt flag
        }

        // Print that logging has finished
        System.out.println("Logging FINISHED for: " + message + " | by " + Thread.currentThread().getName());
    }
}
```

---

### 🚀 `AuditSimulationApp.java`

```java
package com.example.audit;

import java.util.Arrays;
import java.util.List;

/**
 * Main class to simulate audit trail logging using threads.
 *
 * Theory:
 * In real systems, audit logs are often handled by a logging framework
 * or background thread to avoid blocking user-facing processes.
 */
public class AuditSimulationApp {

    public static void main(String[] args) {

        // Simulated user actions to be logged
        List<String> actions = Arrays.asList(
                "User A logged in",
                "User B transferred $1000",
                "User C logged out",
                "Admin updated system config",
                "User A requested account statement"
        );

        // For each action, create and start a thread with AuditLogger
        for (String action : actions) {
            AuditLogger logger = new AuditLogger(action);

            // Create a uniquely named thread for each action
            Thread logThread = new Thread(logger, "LoggerThread-" + action.hashCode());
            logThread.start();

            // Optional: Uncomment this block if sequential execution is needed for demonstration
            /*
            try {
                logThread.join();
            } catch (InterruptedException e) {
                System.err.println("Main thread interrupted.");
                Thread.currentThread().interrupt();
            }
            */
        }

        // Indicate all threads have been started
        System.out.println("All audit log tasks have been initiated.");
    }
}
```

---

### 🧪 `AuditLoggerTest.java`

See the full JUnit 5 test suite in your last message – it's already comprehensive, documented, and complete. Just make sure it's in the same package (`com.example.audit`) and uses the `JUnit 5` dependency.

---

### 🗂 Recommended Folder Structure

```
src/
├── main/
│   └── java/
│       └── com/
│           └── example/
│               └── audit/
│                   ├── AuditLogger.java
│                   └── AuditSimulationApp.java
└── test/
    └── java/
        └── com/
            └── example/
                └── audit/
                    └── AuditLoggerTest.java
```

---

## 📚 Concepts Students Should Take Away

| Concept               | Application in Code                                     |
| --------------------- | ------------------------------------------------------- |
| Runnable              | Defined `AuditLogger` task as a Runnable                |
| Thread                | Created per-log-entry threads to simulate async logging |
| Thread.sleep()        | Simulates delay to mimic I/O latency                    |
| join()                | (Optional) Waits for threads to complete                |
| System.out            | Temporary logging output for simulation                 |
| ByteArrayOutputStream | Used in tests to capture and verify output              |
| JUnit 5               | Used to write unit and concurrency tests cleanly        |

---

## ✅ Submission Instructions

1. ✅ Complete the TODOs in both classes (`AuditLogger`, `AuditSimulationApp`)
2. ✅ Run the test suite in `AuditLoggerTest.java` to confirm functionality
3. ✅ Push your code to GitHub under a `solution` branch
4. ✅ Share your branch URL in your submission

