

### Section 1: Swift Language Proficiency (60 Questions)

#### Value vs. Reference Types (`struct` vs. `class`)
1.  What is the fundamental difference between a `struct` and a `class`?
2.  Explain value semantics vs. reference semantics.
3.  When would you explicitly choose to use a `struct` over a `class`? Provide three distinct examples.
4.  When is a `class` the more appropriate choice over a `struct`?
5.  How does the memory allocation differ for `structs` and `classes` (stack vs. heap)?
6.  What is Copy-on-Write (CoW)? How does Swift's standard library use it?
7.  Can a `struct` have inheritance? How can you achieve similar functionality?
8.  How do value and reference types impact performance and thread safety?
9.  Explain how passing a `struct` to a function differs from passing a `class` instance.
10. Can you define methods within a `struct`? What is the purpose of the `mutating` keyword?

---

#### Memory Management & ARC
11. What is ARC and how does it manage memory in Swift?
12. Explain the difference between `strong`, `weak`, and `unowned` references.
13. What is a retain cycle (strong reference cycle)?
14. Provide a code example of a retain cycle between two classes and explain how to break it.
15. How can closures create retain cycles?
16. What is a capture list in a closure and how does it help prevent retain cycles?
17. When would you use `[unowned self]` instead of `[weak self]` in a capture list? What are the risks?
18. Does capturing `self` in a GCD `async` block always cause a retain cycle? Explain why or why not.
19. What is a dangling pointer, and which reference type can lead to one?
20. How do `weak` references work internally? (Hint: side tables).

---

#### Optionals
21. What is an optional in Swift? What problem does it solve?
22. Describe three different ways to safely unwrap an optional.
23. What is the difference between `if let` and `guard let`? When is `guard let` more appropriate?
24. Explain optional chaining. Provide a code example.
25. What is the nil-coalescing operator (`??`)?
26. What is an implicitly unwrapped optional? When might you see it used, and what are its dangers?
27. What happens when you force unwrap an optional that is `nil`?

---

#### Protocols & Generics
28. What is Protocol-Oriented Programming (POP)? How does it differ from Object-Oriented Programming (OOP)?
29. Can you add default method implementations to a protocol? How?
30. What are associated types in protocols?
31. How can you constrain a generic type to a specific protocol?
32. What problem do generics solve in Swift?
33. Can you create a generic function that works on any type conforming to `Equatable`?
34. What is the difference between `Any` and `AnyObject`?
35. How can you make a protocol's methods optional? (Hint: `@objc` or protocol extensions).

---

#### Closures & Higher-Order Functions
36. What is a closure in Swift?
37. What does the `@escaping` attribute mean for a closure? When is it required?
38. Explain the difference between `map`, `flatMap`, and `compactMap`.
39. Provide an example of using `filter` on an array of integers.
40. What is the `reduce` function used for? Provide a simple example.

---

#### Error Handling
41. Describe Swift's primary error handling model (`throws`, `do-catch`).
42. What is the difference between `try`, `try?`, and `try!`?
43. How do you define a custom error type in Swift?
44. What is the `Result` type and how is it useful, especially in asynchronous operations?
45. When is it more appropriate to return an optional versus throwing an error?
46. What does the `rethrows` keyword signify?
47. Can a `catch` block handle specific types of errors? How?

---

#### Other Swift Concepts
48. What is the difference between `let` and `var`?
49. Explain the difference between a computed property and a stored property.
50. What are property observers (`willSet`, `didSet`)?
51. What is a `lazy` stored property? When is it useful?
52. What is type inference?
53. Explain access control in Swift (`private`, `fileprivate`, `internal`, `public`, `open`).
54. What is the `defer` statement used for?
55. What is the difference between `static` and `class` keywords on methods or properties?
56. What is ABI Stability and why was it a significant milestone for Swift?
57. How does key-value observing (KVO) work in Swift?
58. What are key paths?
59. What is the difference between `==` and `===`?
60. How do you conform a custom type to the `Equatable` and `Hashable` protocols?

***

### Section 2: UI Layer (UIKit & SwiftUI) (65 Questions)

#### Lifecycles
61. Describe the states of the iOS application lifecycle.
62. Which `AppDelegate` methods correspond to these state transitions?
63. Explain the key methods of the `UIViewController` lifecycle in order.
64. What is the difference between `loadView` and `viewDidLoad`?
65. When is `viewWillAppear` called versus `viewDidAppear`? What kind of tasks are suitable for each?
66. If you push a new view controller onto a navigation stack, which lifecycle methods are called on the old and new controllers?
67. What is the significance of `didReceiveMemoryWarning`?
68. Describe the lifecycle of a SwiftUI view.
69. What are the purposes of `onAppear` and `onDisappear` in SwiftUI?

---

#### Layout & Views (UIKit)
70. What is the difference between a view's `frame` and its `bounds`?
71. What is Auto Layout?
72. How do you resolve an ambiguous or conflicting layout in Auto Layout?
73. What is the intrinsic content size of a view?
74. What is `UIStackView` and what are its main properties (`axis`, `distribution`, `alignment`, `spacing`)?
75. What is the responder chain? How are touch events handled?
76. What is the difference between `UIView` and `CALayer`?
77. How can you create a custom `UIView` subclass?
78. What are Size Classes and how do they help in creating adaptive UIs?
79. How do you handle keyboard appearance and dismissal to prevent it from covering a `UITextField`?
80. What is the purpose of `layoutIfNeeded`, `setNeedsLayout`, and `setNeedsDisplay`?

---

#### `UITableView` & `UICollectionView`
81. How does `UITableView` optimize memory usage for a large number of rows?
82. What is the purpose of a `reuseIdentifier`?
83. What is the difference between `UITableView` and `UICollectionView`?
84. How would you implement a cell with a dynamic height in `UITableView`?
85. Describe how to optimize scrolling performance in a `UITableView` or `UICollectionView`.
86. What is the `UITableViewDataSourcePrefetching` protocol used for?
87. How do you handle user selections on a `UITableView` cell?
88. Explain the roles of `dataSource` and `delegate` in `UITableView`.
89. How would you implement multiple cell types in a single `UITableView`?

---

#### SwiftUI
90. What is the core difference between the imperative approach of UIKit and the declarative approach of SwiftUI?
91. What is a `View` in SwiftUI? Explain the `some View` return type.
92. What is the purpose of `@State`? When should it be used?
93. Explain `@Binding`. How does it create a two-way connection?
94. What is the difference between `@ObservedObject` and `@StateObject`? Which one should you use to instantiate a view model?
95. What is `@EnvironmentObject` and when is it useful?
96. How do you manage navigation in a SwiftUI app?
97. What are `VStack`, `HStack`, and `ZStack`?
98. How can you create a list of dynamic content in SwiftUI?
99. What is a `ViewModifier`? Provide an example of creating a custom one.
100. What is the purpose of `@ViewBuilder`?
101. How do you handle asynchronous tasks, like a network call, when a SwiftUI view appears?
102. What is the role of the Combine framework in SwiftUI?
103. How do you integrate a UIKit `UIView` or `UIViewController` into a SwiftUI view?
104. How do you embed a SwiftUI view within a UIKit application?
105. How does SwiftUI handle orientation or screen size changes?
106. What is `GeometryReader` and what are its potential performance implications?
107. How do you handle user input and gestures in SwiftUI?
108. What triggers a view to re-render in SwiftUI?
109. What are the pros and cons of using SwiftUI over UIKit for a new project?
110. How do you implement animations in SwiftUI?

---

#### Navigation
111. What is a `UINavigationController`? How does it manage its stack of view controllers?
112. What is the difference between `present()` and `pushViewController()`?
113. What is a segue in the context of Storyboards?
114. How do you pass data between view controllers when using segues?
115. What is a `UITabBarController`?
116. How do you programmatically navigate between views in SwiftUI?
117. What is the difference between a modal presentation and a push navigation?
118. How would you implement a custom view controller transition?
119. What is the role of a Coordinator in navigation?
120. How do you handle deep linking into a specific screen in your app?
121. What is the purpose of `unwind` segues?
122. How does `NavigationView` work in SwiftUI?
123. What is the difference between a sheet and a full-screen cover in SwiftUI?
124. How can you dismiss a modally presented view controller?
125. What is the purpose of `prepare(for:sender:)`?

***

### Section 3: Concurrency (45 Questions)

#### GCD (Grand Central Dispatch)
126. What is Grand Central Dispatch (GCD)?
127. What is the difference between a serial queue and a concurrent queue?
128. What is the difference between `sync` and `async` dispatch?
129. What is the main dispatch queue? Why is it important for UI updates?
130. What is a deadlock? Provide a GCD code example that would cause a deadlock.
131. How can you avoid deadlocks when using GCD?
132. What is Quality of Service (QoS) in GCD? Name the different QoS classes.
133. What is a `DispatchGroup` and when would you use it?
134. What is a `DispatchWorkItem`? How can it be used to cancel a task?
135. What is a dispatch barrier? What problem does it solve?
136. Explain the "readers-writers" problem and how you would solve it using GCD.
137. What is a semaphore (`DispatchSemaphore`)? Give a use case.
138. Is a dispatch queue a thread? Explain the relationship.
139. How would you perform a task after a delay using GCD?
140. When should you create a custom queue versus using a global queue?

---

#### `Operation` & `OperationQueue`
141. What is an `Operation` and `OperationQueue`?
142. What are the advantages of using `OperationQueue` over GCD?
143. How do you set up dependencies between `Operation`s?
144. How can you cancel an `Operation`? Can all operations be canceled?
145. How do you control the maximum number of concurrent operations in an `OperationQueue`?
146. What is the difference between `BlockOperation` and creating a custom `Operation` subclass?
147. How can you observe the state of an `Operation` (e.g., `isFinished`, `isExecuting`)?
148. When would you still choose to use GCD over `OperationQueue`?

---

#### Swift Concurrency (`async/await`, Actors)
149. What is `async/await` in Swift?
150. How does `async/await` improve upon completion handlers?
151. What is Structured Concurrency?
152. How do you mark a function as `async`?
153. How do you handle errors in an `async` function?
154. What is a `Task`? How do you create one?
155. What is the difference between `async let` and awaiting tasks sequentially?
156. What is a `TaskGroup`? When is it useful?
157. How does cancellation work in Swift Concurrency?
158. What is an `Actor`? What problem does it solve?
159. How do actors prevent data races?
160. What is `@MainActor`? Why is it important?
161. How can you call an `async` function from a synchronous context?
162. What is a continuation (`withCheckedContinuation`)? When might you need it?
163. Can `async/await` be used with Combine? How?
164. What are some common pitfalls when using `async/await`?
165. What is a data race?
166. How do you test `async` code using `XCTest`?
167. What are global actors?
168. Explain the difference between a `Task` and a detached `Task`.
169. How does Swift's concurrency model manage threads?
170. What is the `await` keyword used for in actor methods?

***

### Section 4: Architecture & System Design (40 Questions)

#### Architectural Patterns
171. What is MVC (Model-View-Controller)? What are its responsibilities?
172. What is the "Massive View Controller" problem in MVC?
173. Explain the MVVM (Model-View-ViewModel) pattern.
174. How does MVVM help solve the Massive View Controller problem?
175. What is the role of data binding in MVVM?
176. Compare MVC and MVVM. What are the pros and cons of each?
177. What is the VIPER architecture? Name its five components.
178. What are the main advantages and disadvantages of VIPER?
179. When would a project benefit from using VIPER over MVVM?
180. Explain the MVP (Model-View-Presenter) pattern. How does it differ from MVC and MVVM?
181. How do you handle navigation/routing in MVC, MVVM, and VIPER?
182. Which architectural pattern is most suitable for SwiftUI? Why?
183. How would you decide which architecture to use for a new project?

---

#### Design Patterns
184. What is the Singleton pattern? Provide an example from the iOS SDK.
185. What are the potential drawbacks of using Singletons?
186. What is the Delegate pattern?
187. Why should a delegate property almost always be declared as `weak`?
188. What is the Observer pattern? How is it implemented in iOS (e.g., `NotificationCenter`, KVO)?
189. Compare the Delegate pattern with `NotificationCenter`. When would you use one over the other?
190. What is the Factory pattern?
191. What is the Coordinator pattern? What problem does it solve?
192. What is Dependency Injection? Why is it beneficial for testing?

---

#### Client-Side System Design
193. You need to design the client-side architecture for a news feed app (like Instagram). What are the key components you would create?
194. How would you design the networking layer for this app?
195. What caching strategy would you implement for the news feed's images?
196. Explain how you would implement "infinite scrolling" for the feed.
197. How would you handle offline support for the news feed?
198. What data models would you need for a news feed?
199. How would you handle API pagination (e.g., cursor-based vs. page-based)?
200. Describe how you would implement "optimistic updates" for an action like "liking" a post.
201. What are the main performance considerations for a news feed? (e.g., battery, data usage).
202. How would you design a robust image caching system with both memory and disk caches?
203. What is an LRU (Least Recently Used) cache eviction policy?
204. How would you design a modular app architecture?
205. What API endpoints would you expect the backend to provide for a news feed?
206. How would you manage different environments (e.g., development, staging, production)?
207. What security considerations are important when designing a client-side app?
208. How would you design a feature that requires real-time updates (e.g., a chat messenger)?
209. What are the trade-offs between fetching all data upfront vs. loading it on demand?
210. How do you approach making architectural decisions for a new project?

***

### Section 5: Data & Networking (35 Questions)

#### Persistence
211. Compare `UserDefaults`, Keychain, and Core Data. When should each be used?
212. What kind of data is suitable for `UserDefaults`? What are its limitations?
213. Why should you never store sensitive information in `UserDefaults`?
214. What is the Keychain used for? How does it provide security?
215. What is Core Data? Is it a database?
216. What are the main components of a Core Data stack? (e.g., `NSManagedObjectContext`, `NSPersistentStoreCoordinator`).
217. What is an `NSManagedObject`?
218. How do you fetch data from Core Data using `NSFetchRequest`?
219. What is an `NSPredicate`?
220. What are the pros and cons of using Core Data vs. Realm or raw SQLite?
221. How can you store a custom `struct` in `UserDefaults`? (Hint: `Codable`).
222. How does data persistence change in the Keychain when an app is uninstalled?
223. How would you handle Core Data migrations?
224. What is the purpose of an `NSManagedObjectContext`? Can you have multiple contexts?
225. How would you perform Core Data operations on a background thread?

---

#### Networking
226. What is `URLSession`? What are its main components?
227. How do you make a GET request using `URLSession`?
228. How do you make a POST request with a JSON body?
229. What is the `Codable` protocol? How does it simplify JSON parsing?
230. How would you parse a complex, nested JSON object using `Codable`?
231. What if the JSON keys don't match your Swift model's property names? How do you handle that with `Codable`?
232. How do you handle different date formats when decoding JSON?
233. What is the difference between `URLSession`'s data task, download task, and upload task?
234. How do you handle networking errors with `URLSession`?
235. What is App Transport Security (ATS)?
236. How would you implement lazy loading of images from a URL?
237. What is SSL pinning and why might it be used?
238. How would you structure a networking layer in an application for reusability and testability?
239. What are HTTP status codes? Name a few common ones (e.g., 200, 404, 500).
240. How do you set custom HTTP headers on a `URLRequest`?
241. What are the different `URLSessionConfiguration` types (`default`, `ephemeral`, `background`)?
242. How do you handle authentication tokens in your network requests?
243. What is the purpose of the `Result` type in a networking completion handler?
244. How would you debug network requests from your app?
245. What are some best practices for networking on mobile to conserve battery and data?

***

### Section 6: Engineering Quality & Tools (55 Questions)

#### Testing
246. What is the difference between a unit test and a UI test?
247. What is the `XCTest` framework?
248. How do you set up a unit test target in Xcode?
249. Explain the lifecycle of an `XCTestCase` (`setUp`, `tearDown`).
250. What are test doubles? Explain the difference between mocks and stubs.
251. Why are mocks and stubs essential for effective unit testing?
252. How would you unit test a view model that has a networking dependency?
253. What is Test-Driven Development (TDD)? What are its benefits?
254. What is code coverage? How do you enable it in Xcode?
255. How do you write a test for an asynchronous function using `XCTestExpectation`?
256. What is `XCUITest`?
257. How do you find UI elements in an `XCUITest`?
258. What are some best practices for writing clean and maintainable tests?
259. How do you handle elements that might not exist yet in a UI test?
260. What is a performance test in `XCTest`?

---

#### Debugging & Profiling
261. What tools would you use to debug a memory leak in your application?
262. How does the Leaks instrument work?
263. How would you use the Allocations instrument and memory graph debugger to find a retain cycle?
264. Your app's UI is stuttering during scrolling. Which instrument would you use to diagnose the problem?
265. What is the Time Profiler instrument used for?
266. How do you use breakpoints effectively in Xcode? What is a conditional breakpoint?
267. What is the view hierarchy debugger?
268. How would you analyze your app's launch time performance?
269. What are some common causes of poor app performance?
270. How do you use the Xcode Analyze tool? What kind of issues can it find?

---

#### Dependency Management
271. What is a dependency manager?
272. Compare CocoaPods and Swift Package Manager (SPM).
273. What are the advantages of using SPM over CocoaPods?
274. How do you add a package dependency using SPM in Xcode?
275. What is a `Podfile`? What is a `Podfile.lock`?
276. Why is it important to commit the `Podfile.lock` (or `Package.resolved`) file to source control?
277. What is the difference between a static and a dynamic library?

---

#### CI/CD & App Store
278. What is Continuous Integration (CI)?
279. What is Fastlane and what can it be used for?
280. Describe the general process of releasing an app to the App Store.
281. What is TestFlight used for?
282. What is a provisioning profile?
283. What is a code signing certificate?
284. Your app was rejected by Apple. What are your next steps?
285. How do you handle app versioning (e.g., semantic versioning)?

---

#### General Engineering & Soft Skills
286. How do you stay updated with the latest developments in iOS and Swift?
287. Describe your process for a code review. What do you look for?
288. You've discovered a critical bug in production. How do you handle it?
289. How would you approach refactoring a large, legacy codebase?
290. Describe a challenging technical problem you solved on a previous project.
291. How do you balance writing high-quality code with meeting deadlines?
292. What are your thoughts on pair programming?
293. How do you approach learning a new technology or framework?
294. How do you ensure your app is accessible?
295. What is your experience with source control systems like Git?
296. Explain a situation where you had a disagreement with a team member on a technical decision and how you resolved it.
297. What is technical debt? How do you manage it?
298. How do you handle user feedback and App Store reviews?
299. What makes an app have a great user experience, in your opinion?
300. What questions do you have for us about our team, process, or technology stack?