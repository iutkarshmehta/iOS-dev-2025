# 📘 Apple Developer Documentation: XPC

## I. Overview of the XPC Framework

XPC is a lightweight, low-level inter-process communication (IPC) framework developed by Apple for its operating systems, including macOS and iOS.[1, 2, 3] It provides a structured and secure mechanism for applications to communicate with helper tools, known as XPC services. These services run as separate processes, managed by the system's `launchd` daemon, which launches them on demand, shuts them down when idle, and restarts them if they crash.[1, 3]

The primary goals of XPC are to enhance application **stability** and **security**. By isolating work into separate processes, a crash in an XPC service will not bring down the main application. Furthermore, XPC is a cornerstone of Apple's security model, enabling **privilege separation**. Each service can be sandboxed with the minimal set of permissions required for its specific task, drastically limiting the potential damage if that service is compromised.[3, 4, 5]

## II. 🎯 Core Architecture and Concepts

XPC operates on a client-server model, where a "listener" (the server) responds to connection requests and performs tasks on behalf of a "client".[1] This entire lifecycle is orchestrated by `launchd`, the system's core process manager.[6]

### A. The Role of `launchd`

`launchd` is the first process to run on the system and is responsible for managing all other daemons, agents, and services, including XPC services.[6, 7] It launches XPC services on-demand when a client first attempts to connect, ensuring that resources are used efficiently. When a service is no longer in use, `launchd` can terminate it to free up memory.[3, 8] This on-demand lifecycle is a key feature of the XPC architecture.

### B. Types of XPC Services

XPC services can be categorized by their scope and how they are managed by `launchd`. The type of service determines its process environment and accessibility.[1, 4]

| Service Type | Scope & Environment | Use Case |
| :--- | :--- | :--- |
| **XPC Service (Bundled)** | Runs as one process per client, tied to the client's lifetime. Bundled within the main application's `.app` package. | The most common type, used for breaking an application into smaller, sandboxed components for stability and privilege separation.[1, 3, 8] |
| **Launch Agent** | Runs as one process per logged-in user, under that user's identity. Can interact with the GUI. | Per-user background tasks or helper tools that need to run independently of a specific app but within a user's session.[1, 7] |
| **Launch Daemon** | Runs as a single, system-wide process, typically as the `root` user. Cannot interact with the GUI. | System-level services that require elevated privileges and must be available to all users, such as a privileged helper tool for software installation.[4, 6, 7] |

## III. 📊 Key APIs and Implementation

Apple provides multiple API layers for working with XPC, catering to different needs and programming languages. The choice of API depends on whether the project uses the Foundation framework.[1, 3]

  * **High-Level (Foundation): `NSXPCConnection`**

      * An object-oriented API for Objective-C and Swift projects.
      * Provides a transparent remote procedure call (RPC) mechanism, making it feel like calling methods on a local object.[3, 9, 10]
      * Recommended for most applications due to its ease of use and strong typing.

  * **Low-Level (libSystem): C API and Swift API**

      * A lower-level API for projects that cannot or do not link against Foundation.[1, 3]
      * The original C API (`xpc_connection_t`, `xpc_object_t`) is powerful but more complex.[3]
      * As of macOS 14 and iOS 17, a modern, native Swift API (`XPCSession`, `XPCListener`) has been introduced to provide a safer and more idiomatic alternative to the C API.[1, 2]

### A. Implementing with `NSXPCConnection` (High-Level)

The high-level API is the most common and recommended approach. It revolves around a few key classes and a protocol-based design.[9, 10]

1.  **Define a Protocol:** The contract between the client and the service is defined in a Swift or Objective-C `@protocol`. This protocol lists the methods the service will expose to the client.[10, 11]
2.  **Create the Service-Side Listener:** The XPC service implements a delegate that conforms to `NSXPCListenerDelegate`. When a new connection arrives, the `listener(_:shouldAcceptNewConnection:)` method is called. Inside this method, the service configures the connection by setting its `exportedInterface` (based on the protocol) and its `exportedObject` (the object that actually implements the protocol).[9, 10]
3.  **Establish the Client-Side Connection:** The main application creates an `NSXPCConnection` instance, specifying the service by its bundle identifier (`serviceName`) or Mach service name (`machServiceName`).[9, 12] The client configures the connection's `remoteObjectInterface` to match the service's protocol.
4.  **Communicate via Proxy:** The client obtains a proxy object by calling `remoteObjectProxyWithErrorHandler`. Calling methods on this proxy object transparently sends a message to the service, which executes the corresponding method on its exported object.[9, 11]

💡 **Code Example: Client Connection (Swift)** [11, 13]

```swift
// 1. Create a connection to the service using its bundle ID.
let connection = NSXPCConnection(serviceName: "com.example.MyService")

// 2. Configure the remote interface with the shared protocol.
connection.remoteObjectInterface = NSXPCInterface(with: MyServiceProtocol.self)

// 3. Resume the connection to allow messages to be sent.
connection.resume()

// 4. Get a proxy object to make calls on.
if let service = connection.remoteObjectProxyWithErrorHandler({ error in
    print("Received error:", error)
}) as? MyServiceProtocol {
    // 5. Call a method on the service.
    service.performCalculation(firstNumber: 10, secondNumber: 32) { result in
        print("Result from service: \(result)")
    }
}
```

### B. Privilege Separation in Practice

The core security benefit of XPC is privilege separation. An application can be designed so that the main process, which handles the user interface, runs with very few permissions (e.g., no network or file system access). When it needs to perform a privileged operation, it communicates with a dedicated XPC service that has only the specific entitlement required for that one task.[4, 5]

For example, an app that needs to access the user's contacts and also connect to a web server could be split into two XPC services:

  * **Main App:** No entitlements. Handles UI only.
  * **Contacts Service:** Has the Contacts entitlement. Its sole job is to fetch contact data when requested by the main app.
  * **Networking Service:** Has the Network Client entitlement. Its sole job is to perform network requests.

If a vulnerability is found in the networking code, an attacker would only gain control of the networking service's process, which is sandboxed and has no access to the user's contacts, thereby containing the breach.[14]

## IV. 🔄 Discrepancy and API Lifecycle Report

The XPC framework has evolved over time, with Apple introducing new APIs and deprecating older ones to improve security and developer experience.

### 🆕 Newly Added APIs

  * **Swift-native Low-Level API (macOS 14+, iOS 17.4+):** Apple has introduced a new, modern Swift API for low-level XPC communication.
      * `XPCListener`: A Swift-native type for creating an XPC server.[1]
      * `XPCSession`: A Swift-native type for creating a client connection.[1]
      * These are intended to provide a safer, more idiomatic alternative to the legacy C-based `xpc_listener_t` and `xpc_session_t` types, which are now marked as deprecated in favor of their Swift counterparts.[2]

### 🗑️ Deprecated APIs

  * **`NSMachBootstrapServer`:** This class, which was part of a legacy approach to creating Mach services, was deprecated in macOS 10.13. Developers are now directed to use `NSXPCListener` with a Mach service name (`initWithMachServiceName:`) for creating daemons and agents.[15]
  * **Legacy C-based `xpc_session_t` and `xpc_listener_t`:** With the introduction of the new Swift API, the corresponding C types have been marked as deprecated in recent OS versions (macOS 14, iOS 17) and renamed to `XPCSession` and `XPCListener` respectively.[2] This signals a clear direction towards the modern Swift API for new low-level XPC development.