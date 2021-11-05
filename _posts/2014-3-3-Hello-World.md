---
layout: post
title: Swift Protocols: Properties distinction(get, get set)🏃🏻‍♀️🏃
---

[![Chetan Aggarwal](https://miro.medium.com/fit/c/96/96/1*jcaljTwAZ5cKY91bZFHq5A.jpeg)](https://medium.com/@chetan15aga?source=post_page-----32a34a7f16e9--------------------------------)

I would also suggest reading [Introduction to POP](https://www.raywenderlich.com/148448/introducing-protocol-oriented-programming) by [Niv Yahel](https://www.raywenderlich.com/u/nivivon), a Raywenderlich tutorial, which explains and provide hands-on in the playground too.

## Protocols

A *protocol* defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. The protocol can then be *adopted* by a class, structure, or enumeration to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to *conform* to that protocol. [Apple Documents.](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html)

```swift
protocol SomeProtocol {  
  var mustBeSettable: Int { get set }  
  var doesNotNeedToBeSettable: Int { get }  
}
```

## Protocol Property Declaration

Protocols declare that conforming types must implement a property by including a [*protocol property declaration*](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Declarations.html) in the declaration body. It have a special form of a variable declaration:

```swift
var propertyName:Type {get set}
```

As with other protocol member declarations, these property declarations declare only the getter and setter requirements for types that conform to the protocol. As a result, you don’t implement the getter or setter directly in the protocol in which it is declared.

The getter and setter requirements can be satisfied by a conforming type in a variety of ways. If a property declaration includes both the `get` and `set` keywords, a conforming type can implement it with a stored variable property or a computed property that is both readable and writeable (that is, one that implements both a getter and a setter). However, that property declaration can’t be implemented as a constant property or a read-only computed property. If a property declaration includes only the `get` keyword, it can be implemented as any kind of property. For examples of conforming types that implement the property requirements of a protocol, see [Property Requirements](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Protocols.html#//apple_ref/doc/uid/TP40014097-CH25-ID269).

-   **For** ***Gettable*** **Protocol Properties** , the requirement can be satisfied by **any kind of property**, and it is valid for the property to be also settable if this is useful for your own code.
-   **For *Gettable & Settable* Protocol Property**, the requirement cannot be fulfilled by a constantly stored property or a read-only computed property.
-   Property requirements in a protocol are always declared as variable properties because **they are declared as computed properties.**

## Examples

1.  **Gettable — Constant Property**

```swift
protocol FullyNamed {  
 var fullName: String { get }  
}  
struct Detective: FullyNamed {  
 let fullName: String  
}  
let hercule = Detective(fullName: “Hercule Poirot”)  
print(hercule.fullName) // returns “Hercule Poirot”
```

**2\. Getable — Variable Property**
```swift
protocol FullyNamed {  
 var fullName: String { get }  
}  
struct Detective: FullyNamed {  
 var fullName: String  
}  
var bond = Detective(fullName: “Bond”)  
print(bond.fullName) // returns “Bond”  
bond.fullName = “James Bond”  
print(bond.fullName) // returns “James Bond”
```
**3\. Getable — Computed Property**
```swift
protocol FullyNamed {  
 var fullName: String { get }  
}  
struct Detective: FullyNamed {  
 fileprivate var name: String  
 var fullName: String {  
 return name  
 }  
}  
let batman = Detective(name: “Bruce Wayne”)  
print(batman.fullName) // returns “Bruce Wayne”
```
**4\. Gettable — Private Set**
```swift
protocol FullyNamed {  
    var fullName: String { get }  
}  
public struct Detective: FullyNamed {  
    public private(set) var fullName: String  
    public init(fullName: String) {  
        self.fullName = fullName  
    }     
    public mutating func renameWith(fullName: String) {  
        self.fullName = fullName  
    }  
}  
var holmes = Detective(fullName: "Holmes")  
print(holmes.fullName) // returns "Holmes"  
holmes.renameWith(fullName: "Sherlock Holmes")  
print(holmes.fullName) // returns "Sherlock Holmes"
```
**5\. Gettable & Settable — Computed Property**
```swift
protocol FullyNamed {  
    var fullName: String { get }  
}  
struct Detective: FullyNamed {  
    fileprivate var name: String  
    var fullName: String {  
        get {  
            return name  
        }  
        set {  
            name = newValue  
        }  
    }  
}  
var Payne = Detective(name: "Payne")  
print(Payne.fullName) // returns "Payne"  
Payne.fullName = "Max Payne"  
print(Payne.fullName) // returns "Max Payne"
```
**6\. Gettable & Settable — Constant Property**
```swift
protocol FullyNamed {  
 var fullName: String { get set }  
}  
struct Detective: FullyNamed {  
 let fullName: String  
}  
let Rorschach = Detective(fullName: “Walter Joseph Kovacs”)  
// Error message: Type ‘Detective’ does not conform to protocol ‘FullyNamed’
```
**7\. Gettable & Settable — only Get Defined**
```swift
protocol FullyNamed {  
 var fullName: String { get set }  
}  
struct Detective: FullyNamed {  
 private var name: String  
 var fullName: String {  
 return name  
 }  
}  
var constantine = Detective(name: “John Constantine”)  
// Error message: Type ‘Detective’ does not conform to protocol ‘FullyNamed’
```
## Type casting with protocols:

Get Property in protocols, does not force the conforming type to declare the get property only, it can declare it as **Set** property also, as explained above. Check this example for more clarity.
```swift
protocol FullyNamed{  
 var firstName: String {get}  
 var lastName: String {get set}  
}  
struct SuperHero: FullyNamed{  
 var firstName = “Super”  
 var lastName = “Man”  
}  
var dcHero = SuperHero()  
print(dcHero) // SuperHero(firstName: “Super”, lastName: “Man”)  
dcHero.firstName = “Bat”  
dcHero.lastName = “Girl”  
print(dcHero) // SuperHero(firstName: “Bat”, lastName: “Girl”)
```
Even though the FullyName protocol declares firstName as get only, still we can change its value. This is because FullyName does not stop the conforming type to set the property  
If we explicitly typecast the dcHero to FullyNamed, then compiler won’t allow us to set the firstName, since it has no knowledge about the setter.
```swift
var anotherDcHero:FullyNamed = SuperHero()  
print(anotherDcHero)  
anotherDcHero.firstName = “Bat”   
//ERROR: cannot assign to property: ‘firstName’ is a get-only property  
anotherDcHero.lastName = “Girl”  
print(anotherDcHero)
```
Above declaration, give compile time error while setting firstName. This particular example is explained in detail [here](https://vishal-singh-panwar.github.io/SwiftProtocols/).

Thanks for reading!!!

**If you have any questions please don’t hesitate to leave a comment. Any kind of discussion is also welcome! Happy Coding!!! 💚💚💚💚💚💚**
