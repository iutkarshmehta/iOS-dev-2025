# 📘 Apple Developer Documentation: Keychain Services

## I. Overview of Keychain Services

Keychain Services is Apple's dedicated framework for securely storing small amounts of sensitive user data. It provides applications with a mechanism to save confidential information—such as passwords, cryptographic keys, credit card numbers, certificates, and secure notes—into an encrypted database known as a keychain.[1, 2] By leveraging the keychain, developers can free users from the burden of remembering complex credentials for various services, thereby encouraging better security practices.[1, 3]

The framework, part of the broader `Security` framework, is designed with robust security principles [4]:
*   **Encryption:** All data stored in the keychain is automatically encrypted by the operating system, protecting it from unauthorized access.[3]
*   **Isolation:** On iOS, an application can only access its own keychain items or those explicitly shared with other applications within a designated app group. This sandboxing prevents apps from accessing each other's sensitive data.[4, 5]
*   **Thread Safety:** All keychain operations are guaranteed by Apple to be thread-safe, simplifying concurrent programming.[3]

While macOS supports the management of multiple keychain files, iOS abstracts this away, providing each app with access to a single, unified keychain that is automatically unlocked when the user unlocks their device.[5, 6]

## II. 🎯 Core Concepts: Keychain Items and Attributes

Interaction with the keychain is performed by creating, querying, and manipulating **keychain items**. A keychain item is a piece of data packaged with a set of attributes that describe the data and control its accessibility.[4] These attributes are specified in a dictionary when performing any keychain operation.[5]

### A. Item Classes

The most fundamental attribute is the item's class, specified with the `kSecClass` key. It defines the type of data being stored.[7, 8]

| Item Class Constant | Purpose |
| :--- | :--- |
| `kSecClassGenericPassword` | 🎯 For storing generic passwords or any other sensitive data, such as API keys or tokens.[8] |
| `kSecClassInternetPassword` | 🌐 For storing credentials related to internet services, such as websites or APIs. Includes attributes for server, protocol, etc..[8] |
| `kSecClassCertificate` | 📜 For storing X.509 certificates.[8] |
| `kSecClassKey` | 🔑 For storing cryptographic keys.[8] |
| `kSecClassIdentity` | 🆔 For storing an identity, which is a combination of a certificate and its corresponding private key.[8] |

### B. Common Item Attributes

Attributes are used to uniquely identify an item for retrieval and to define its security policies.

| Attribute Constant | Description |
| :--- | :--- |
| `kSecAttrAccount` | A string representing the account name associated with the item (e.g., a username). This is a primary key for identifying an item.[8, 9] |
| `kSecAttrService` | A string that defines the service or application the item belongs to. Often set to the app's bundle identifier to ensure uniqueness.[4, 10] |
| `kSecValueData` | The actual sensitive data to be stored, which must be provided as a `Data` object.[4, 8] |
| `kSecAttrAccessible` | 🛡️ A critical security attribute that specifies when the keychain item can be accessed. See the table below for details.[8, 9] |
| `kSecAttrAccessGroup` | Allows multiple apps from the same developer to share a keychain item. The apps must be signed with the same Team ID and have the proper entitlements.[10, 11] |
| `kSecReturnData` | A boolean value used in search queries. If `true`, the query will return the item's `kSecValueData`.[4, 8] |
| `kSecMatchLimit` | Used in search queries to specify the maximum number of results to return, typically `kSecMatchLimitOne` or `kSecMatchLimitAll`.[4, 8] |

### C. Keychain Item Accessibility Constants

The `kSecAttrAccessible` attribute is one of the most important for security, as it dictates the conditions under which your app can read the stored secret.

| Accessibility Constant | When Data is Accessible |
| :--- | :--- |
| `kSecAttrAccessibleWhenUnlocked` | (Default) The item can only be accessed when the device is unlocked. It becomes inaccessible when the user locks the device.[8] |
| `kSecAttrAccessibleAfterFirstUnlock` | The item is accessible once the device has been unlocked for the first time after a restart. It remains accessible even if the user subsequently locks the device.[8, 11] |
| `kSecAttrAccessibleAlways` | The item is always accessible, even when the device is locked. This is the least secure option.[8, 11] |
| `kSecAttrAccessibleWhenPasscodeSetThisDeviceOnly` | The item is accessible only if the device has a passcode set. The item is not migrated to a new device or included in backups.[8, 11] |
| `...ThisDeviceOnly` (variants) | Variants of the above constants (e.g., `kSecAttrAccessibleWhenUnlockedThisDeviceOnly`) that prevent the item from being transferred to another device via an iCloud Keychain or an encrypted backup.[11] |

## III. 📊 The Core API Functions

The Keychain Services API is a low-level C-based API. All interactions are performed through four primary functions that execute CRUD (Create, Read, Update, Delete) operations. Each function returns an `OSStatus` code to indicate success (`errSecSuccess`) or failure.[3, 5]

| Function | Purpose |
| :--- | :--- |
| `SecItemAdd` | Adds a new item to the keychain. The query dictionary must contain the item's class, attributes, and the data to be stored (`kSecValueData`).[5, 7] |
| `SecItemCopyMatching` | Searches for and retrieves one or more items from the keychain. The query dictionary specifies the search criteria and what should be returned (e.g., `kSecReturnData`).[5, 7] |
| `SecItemUpdate` | Modifies the attributes of an existing keychain item. This function takes two dictionaries: one to find the item and another containing the attributes to update.[5, 7] |
| `SecItemDelete` | Removes an item from the keychain. The query dictionary specifies which item to delete.[5, 7] |

## IV. 🔄 Discrepancy Report

### 🗑️ Deprecated APIs

A significant portion of the Keychain Services API, specifically the functions related to direct management of keychain files on macOS, has been deprecated.

*   **`SecKeychain*` Functions:** The entire family of functions prefixed with `SecKeychain` (e.g., `SecKeychainCreate`, `SecKeychainLock`, `SecKeychainUnlock`, `SecKeychainFindGenericPassword`) is deprecated as of macOS 12.0 and earlier.[12, 13, 14]
*   **Reason for Deprecation:** Apple's official guidance indicates that "Custom keychain management is no longer supported".[14] The modern approach abstracts away direct file manipulation, encouraging developers to work with items within the user's default keychain.
*   **`kSecAttrAccessibleAlways`:** While a WWDC 2015 session noted that this accessibility constant would be deprecated in iOS 9, it was not officially marked as such in the documentation and remains functional. This has been a point of confusion for developers.[15]

### ✅ Modern Approach

The recommended and fully supported method for interacting with Keychain Services on all Apple platforms is to use the **`SecItem*` functions** (`SecItemAdd`, `SecItemCopyMatching`, `SecItemUpdate`, `SecItemDelete`). These functions operate on keychain items within the default keychain, providing a higher-level, more secure, and platform-agnostic API that does not require manual management of keychain files.[3, 5, 9]

https://docs-assets.developer.apple.com/published/ad0bbbff6a49d15c0da8e31ef76adb08/media-2891902%402x.png