# Data-Storage

## UserDefaults
* Most well-known, easiest to use
* Key-Value storage
* For simple user preferences or to store simple values that help improve the user’s experience
* It’s not encouraged to store large blobs of data, images and other large data structures in user defaults
  *(On tvOS, the user defaults store is even limited to a maximum 1MB of data and Apple recommends not exceeding 512KB)*
* You should never store any sensitive data in user defaults. This storage is not encrypted at all
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
## The Keychain
## Databases (like CoreData and SQLite)
