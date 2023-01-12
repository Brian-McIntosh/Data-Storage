# Data-Storage

## UserDefaults
* Most well-known, easiest to use
* Key-Value storage
* For simple user preferences or to store simple values that help improve the user’s experience
* It’s not encouraged to store large blobs of data, images and other large data structures in user defaults
  *(On tvOS, the user defaults store is even limited to a maximum 1MB of data and Apple recommends not exceeding 512KB)*
* You should **never** store any sensitive data in user defaults. This storage is not encrypted at all
* A user’s email address, password and other, similar data, should be stored somewhere more secure
```swift
let defaults = UserDefaults.standard

// SETTING, saving values
defaults.set(25, forKey: "Age")
defaults.set(true, forKey: "UseTouchID")
defaults.set("Paul Hudson", forKey: "Name")

// can store arrays and dictionaries too:
let array = ["Hello", "World"]
defaults.set(array, forKey: "SavedArray")

let dict = ["Name": "Paul", "Country": "UK"]
defaults.set(dict, forKey: "SavedDict")

// GETTING, read values back
let age = defaults.integer(forKey: "Age")
let useTouchID = defaults.bool(forKey: "UseTouchID")
let pi = defaults.double(forKey: "Pi")
```
When retrieving objects, the result is optional. This means you can either accept the optionality, or typecast it to a non-optional type and use the nil coalescing operator to handle missing values. For example:
```swift
let savedArray = defaults.object(forKey: "SavedArray") as? [String] ?? [String]()
```
## Files on disk
* The file system typically has a large amount of storage available
* Can be relatively slow to read large amounts of data from
* Mmages, videos or large JSON files are suited to be stored on disk. These kinds of files are known as binary data
* It's not uncommon for developers to make data structures conform to Codable so they can easily convert their objects to Data, which can then be written to a file on the file system. Using disk storage to store your Codable objects is especially nice if you want to create a cache of responses that you receive from the network
* Without any custom encryption, disk storage is very insecure
* You should avoid storing sensitive data like usernames, passwords, and more in a file that you write to disk at all costs
```swift
do {
  let fileManager = FileManager.default
  let docs = try fileManager.url(for: .documentDirectory,
                                 in: .userDomainMask,
                                 appropriateFor: nil, create: false)
  let path = docs.appendingPathComponent("myFile.txt")
  let data = "Hello, world!".data(using: .utf8)!
  fileManager.createFile(atPath: path.absoluteString, contents: data, attributes: nil)
} catch {
  // handle error
}
```

## The Keychain
* Where iOS stores all kinds of sensitive user data like usernames, passwords, certificates and more
* On iOS, the keychain is probably the best-secured place for you to store data
* Use the keychain to store things like OAuth tokens that authenticate a user against a web service
* Apple is so confident in the keychain that they use it to store username and password combinations
* Non-trivial to navigate and use
  * there are wrappers available on CocoaPods, Carthage and Swift Package Manager that make using the Keychain much, much easier


## Databases (like CoreData and SQLite)
If you have large objects of structured data:
* a user’s to-do list
* data from your user’s weekly running exercises

## Summary
1. Keychain is the most secure place to store user secrets
2. User defaults are great for simple key-value pairs
3. File storage is great for storing large blobs of data
4. Databases should be used to store structured data
