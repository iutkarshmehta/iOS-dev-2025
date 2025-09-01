# 📘 Apple Developer Documentation: SystemConfiguration

## I. Overview of the SystemConfiguration Framework

The System Configuration framework is a foundational component for any application that needs to interact with the device's network settings. It provides a comprehensive set of APIs that allow applications to access network configuration details and, most notably, to determine the reachability of target hosts.[1] Available across all of Apple's platforms—including iOS, macOS, and visionOS—this framework serves as the primary tool for understanding the network state of the device, such as whether Wi-Fi or cellular connectivity is active.[1]

The framework's dual goals are to provide dynamic network configuration that supports seamless user mobility and to offer services for applications that need to create, modify, or access network services.[2] It achieves this by abstracting low-level network events into a structured and accessible format, enabling developers to build network-aware applications that can adapt gracefully to changing conditions.[2, 3]

## II. 🎯 Core Architecture: The Data Stores

The power of the System Configuration framework lies in its sophisticated data management architecture, which is composed of two primary data stores: the **Persistent Store** and the **Dynamic Store**. These components work in tandem to manage network preferences and reflect the live state of the network.[3]

*   **The Persistent Store:** This is a hierarchically structured database that contains all network preferences set by the user or by applications. It holds the configuration information for every location, service, and interface defined on the system, regardless of whether they are currently active. This data is stored in an XML property list file (`preferences.plist`).[3] 📌 **Important:** While its location can be found, developers must never access this file directly; all interactions should occur through the framework's APIs.[3]

*   **The Dynamic Store:** This store provides a real-time snapshot of the current network state. It contains a subset of the information from the Persistent Store—specifically, the preferences for the *currently active* configuration. Configuration agents continuously update the dynamic store in response to system and network events (like an Ethernet cable being unplugged), making it the source of truth for the live network environment.[3]

The structure of both stores is dictated by the **System Configuration Schema**, a low-level but public set of definitions that outlines the precise key-value pairs used to describe all network services and entities.[3]

| Feature | Persistent Store | Dynamic Store |
| :--- | :--- | :--- |
| **Purpose** | Stores all user-defined network configurations. | Reflects the live, real-time state of the network. |
| **Content** | All locations, services, and interfaces (active and inactive). | Only the currently active network configuration and state. |
| **Volatility** | Non-volatile; persists across reboots. | Volatile; represents the current session only. |
| **Updated By** | User actions (in System Settings) and configuration apps. | System configuration agents in response to network events. |
| **Primary Use Case** | Storing long-term network preferences. | Monitoring live network status and reachability. |

## III. 📊 Key APIs and Functionality

The framework exposes several groups of functions and data types for interacting with the system's network configuration.

### A. Network Reachability (Legacy)

Historically, the most widely used feature of the System Configuration framework was the `SCNetworkReachability` API. This interface allowed an application to determine the status of a system's network configuration and the reachability of a target host before attempting a connection.[4] A host was considered "reachable" if a data packet could leave the local device, though this did not guarantee the packet would be received.[4]

The API supported both synchronous checks (using `SCNetworkReachabilityGetFlags`) and asynchronous notifications by scheduling a reachability reference on a run loop or dispatch queue to receive callbacks when the network state changed.[4, 5]

The table below summarizes the key functions of this now-deprecated API for historical context and for maintaining legacy code.

| Function/Method | Description |
| :--- | :--- |
| `SCNetworkReachabilityCreateWithName` | Creates a reachability reference to a specified network host or node name.[6] |
| `SCNetworkReachabilityCreateWithAddress` | Creates a reachability reference to a specified network address. |
| `SCNetworkReachabilityGetFlags` | Synchronously determines if the specified network target is reachable using the current network configuration.[4] |
| `SCNetworkReachabilitySetCallback` | Assigns a client callback to receive notifications when the reachability of the target changes.[4] |
| `SCNetworkReachabilityScheduleWithRunLoop` | Schedules the target on a specific run loop to receive asynchronous notifications.[5] |
| `SCNetworkReachabilitySetDispatchQueue` | Schedules callbacks for the target on a specified dispatch queue.[4] |

### B. Configuration Functions and Constants

Beyond reachability, the framework provides tools for error handling and interacting with specific network services.

*   **Error Handling:** A simple set of functions allows developers to inspect the outcome of framework calls [7]:
    *   `SCError()`: Returns an error or status code from the most recent function call.
    *   `SCErrorString()`: Returns a human-readable string describing a given status or error code.
    *   `SCCopyLastError()`: Returns a `CFError` object with detailed information about the last error.
*   **Captive Network Support:** The framework includes functions for applications that help users authenticate on captive networks (like those at hotels or airports) [8]:
    *   `CNCopySupportedInterfaces()`: Returns the names of network interfaces being monitored for captive portals.
    *   `CNMarkPortalOnline()`: Informs the system that the application has successfully authenticated the device.
    *   `CNMarkPortalOffline()`: Informs the system that the device is not authenticated.

### C. 💡 Constants

The framework defines a vast collection of constants that are used as keys and values within the configuration stores to define network entities, properties, and protocols. These constants provide a standardized vocabulary for describing the network setup.[9]

| Constant Category | Example Constant | Purpose |
| :--- | :--- | :--- |
| **IPv4 Configuration** | `kSCValNetIPv4ConfigMethodManual` | Specifies that the IPv4 interface is configured manually.[9] |
| **Interface Types** | `kSCValNetInterfaceTypeEthernet` | Identifies an interface as an Ethernet port.[9] |
| **Interface Subtypes** | `kSCValNetInterfaceSubTypeL2TP` | Identifies a VPN interface using the L2TP protocol.[9] |
| **PPP Authentication** | `kSCValNetPPPAuthProtocolCHAP` | Specifies the CHAP authentication protocol for a PPP connection.[9] |
| **Proxies** | `kSCPropNetProxiesHTTPUser` | A property key for the username associated with an HTTP proxy.[9] |
| **SMB/NetBIOS** | `kSCValNetSMBNetBIOSNodeTypeBroadcast` | Specifies the broadcast node type for NetBIOS over TCP/IP.[9] |

## IV. 🔄 Discrepancy Report

### 🗑️ Deprecated/Removed APIs

The most significant change to the System Configuration framework is the **deprecation of the entire `SCNetworkReachability` API collection**.[4, 10]

*   **Reason for Deprecation:** Apple's guidance states that network conditions change too frequently for preflight reachability checks to be accurate or useful. A successful reachability check provides no guarantee that a subsequent network connection will succeed, as the network state could change in the intervening milliseconds.
*   **Modern Alternative:** Instead of performing a preflight check, developers are now advised to **attempt the network connection directly**. For scenarios where immediate connectivity is not guaranteed, the recommended approach is to use the `waitsForConnectivity` property on a `URLSessionConfiguration` object. This allows the system to automatically wait for connectivity to become available before proceeding with the request, providing a more reliable and efficient mechanism for handling dynamic network conditions.[4]