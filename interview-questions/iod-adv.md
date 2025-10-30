
### Advanced Swift Language Concepts

#### Generics & Protocols (POP) 🧩
1.  What is **"type erasure"**? Explain a scenario where you'd need to implement it and show how you would do it (e.g., with `AnyPublisher` in Combine or a custom wrapper).
2.  What are **Phantom Types**? How can they be used to add compiler-level safety to your code? Provide a practical example.
3.  Explain the concept of **Generalized Existentials** (`any Protocol`). How does it differ from the older "existential box" approach, and what problems does it solve?
4.  What are some limitations of protocols with **associated types (PATs)**? How do they affect their use in collections?
5.  How does the Swift compiler implement **dynamic vs. static dispatch**? Explain the performance difference and how using `final` or `private` can affect this.

#### Memory & Performance ⚙️
6.  Explain the layout of a `class` instance in memory versus a `struct`. What is the **overhead** of a reference-counted object?
7.  What is an `Autoreleasepool`? While largely managed by ARC, in what specific scenarios might you need to **create one manually**?
8.  What is the **"Throne of Lies"** problem in the context of `unowned` references? Why is `weak` generally safer?
9.  Explain how **Copy-on-Write (CoW)** is implemented. Can you manually implement CoW for your own custom `struct`? How would you do it?

***

### Advanced UI Layer (UIKit & SwiftUI)

#### UIKit Internals 🖼️
10. What is the `RunLoop`? Explain its different modes and how it processes input sources and timers. How can this knowledge be used to **optimize scrolling performance**?
11. What is **offscreen rendering**? Which `CALayer` properties or view configurations can cause it, and how would you use Instruments to detect and fix it?
12. Explain the view **drawing cycle** in detail, from `setNeedsDisplay` to `draw(_:)`. What is the role of `Core Graphics` in this process?
13. How would you build a highly performant, **custom UI control** that requires drawing thousands of elements (e.g., a chart or a map overlay)?

#### SwiftUI Performance & Data Flow ⚡
14. What can cause a SwiftUI view to be **re-evaluated unnecessarily**? How would you use tools like `Self._printChanges()` or Instruments to diagnose and fix these issues?
15. Explain the difference between `@StateObject`, `@ObservedObject`, and `@EnvironmentObject` in terms of their **lifecycle and ownership**. Provide a scenario where misusing one could lead to bugs.
16. How can you use `EquatableView` or the `.equatable()` modifier to **prevent view updates**?
17. How would you implement a **custom `Layout` in SwiftUI**? What are the key methods and concepts involved?
18. Discuss different strategies for managing **complex global state** in a large SwiftUI application. What are the pros and cons of using `EnvironmentObject` vs. a singleton service vs. a dedicated state management library?

***

### Advanced Concurrency

#### Swift Concurrency (`async/await`) Internals 🚀
19. Explain **actor re-entrancy**. What are the potential dangers, and what strategies can you use to avoid them (e.g., state-machine patterns)?
20. What is the `Sendable` protocol and how does the compiler enforce it? What does it mean for a type to be **implicitly `Sendable`**?
21. What is a **custom `Executor`** in Swift Concurrency? Why might you want to create one?
22. How does structured concurrency handle **cancellation propagation** through a `TaskGroup` or `async let` bindings?
23. What is the difference between a `Task` and a `Detached` task in terms of actor context, priority, and lifetime? When is a **detached task** appropriate?

#### GCD & Operations 🚦
24. What is **priority inversion**? How can it occur with GCD queues, and what mechanisms does GCD provide to mitigate it?
25. Explain what a `DispatchSource` is and provide an example of its use (e.g., monitoring a file system event or a signal).
26. When subclassing `Operation`, what are the key properties you must manage to ensure correct **KVO notifications** for its state (e.g., `isExecuting`, `isFinished`)?
27. How would you design a system using `OperationQueue` to handle a complex series of **dependent asynchronous tasks**, where some tasks can run in parallel but must all complete before a final task can begin?

***

### Advanced Architecture & System Design

#### Modularity and Scalability 🏗️
28. How would you design the **dependency graph** for a large, multi-featured app broken into Swift Packages? How would you manage dependencies between features without creating tight coupling?
29. Discuss strategies for **communication between independent modules**. Compare delegates/callbacks, `NotificationCenter`, Combine/Swift Concurrency streams, and using a central Coordinator.
30. You are tasked with designing an **analytics pipeline** for a large app. How would you design it to be type-safe, decoupled from feature modules, and easily testable?
31. How would you design a **feature flagging** or A/B testing system on the client side? How would it integrate with architectural patterns like MVVM or VIPER?

#### Data & Persistence 💾
32. Design a robust, **offline-first data synchronization system**. Discuss your choice of database (e.g., Core Data, GRDB), how you would handle conflict resolution (e.g., Last-Write-Wins, OT), and manage the networking sync logic.
33. What are the challenges of using **Core Data in a multi-threaded environment**? Explain the roles of the main context, background contexts, and how to safely pass data between them.
34. When designing an image caching layer, what are the trade-offs between a simple `NSCache`, a **two-layer cache** (memory + disk), and using a third-party library like Kingfisher or Nuke? How would you handle cache invalidation?

***

### Advanced Engineering Quality & Tools

#### Testing & Automation ✅
35. How do you build a testing strategy for the **"seams" of your architecture** (e.g., the boundary between the UI and ViewModel, or ViewModel and Repository)?
36. How would you write a UI test (`XCUITest`) for a **complex animation** or a gesture-driven interaction to ensure it behaves as expected?
37. How would you set up a **CI/CD pipeline** to automatically run unit tests, UI tests on multiple simulators, and distribute a build to TestFlight upon a merge to the main branch?

#### Debugging & Tooling 🛠️
38. A user reports a crash, and you have a crash log from them. The log is not symbolicated. What is a `dSYM` file, and what is the process you would follow to **symbolicate the crash report** and pinpoint the exact line of code that crashed?
39. How would you use the **command-line tools** available with Xcode (like `xcodebuild` and `xcrun`) to automate parts of your development workflow?
40. Your app's **binary size** is becoming too large. What tools and techniques would you use to diagnose what is contributing to the size and how would you go about reducing it?