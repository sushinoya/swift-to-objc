# Swift to Objective-C

## Main Criteria for comparison
- Working with `nil`
- Limited mutability
- Dynamic vs Static Typing
- Design Patterns and iOS Libraries

### Similarities
- Both have **single inheritance** (i.e a class can inherit from only one other class). So they both use protocols to enable shared functionality across classes.
- Same methods provided by the iOS libraries such as the 6 default methods in AppDelegate.

### Differences
- **Handling `nil`**: Swift uses optionals



 Swift | Objective-C |
| -------- | -------- |
| Swift uses Optionals and only they can be `nil`    | Any object can have a `nil` value. |

Objective-C has no `NullPointerException` or any such consequence of using a nil pointer. 

Eg. 
```objectivec
Person *suyash = nil;
NSLog(@"My name is %@", suyash);

// Output: (null)
```

- **Mutability**

|Swift | Objective-C |
| -------- | -------- |
| Constants are constant, variables vary, and by default we are encouraged to favor constants | All object attributes are mutable by default |

Objective-C classes with limited mutability include all Foundation collections. For example, `NSStrings` are not mutable; to make a change to a string one must either make a new string or use `NSMutableString`. The same goes for `NSArray` and `NSMutableArray`, `NSDictionary` and `NSMutableDictionary`, `NSSet` and `NSMutableSet`, and the remaining classes which you can read about here.

Swift favours use of value types like structs while Objective-C favours reference types like classes.

- **Static vs Dynamic Typing**



 Swift | Objective-C |
| -------- | -------- |
 Static Typing  | Both Static and Dynamic Typing |

In Objective-C, the type of every object needs to be declared but sometimes, the type declared is `id`. `id` can take on any type essentially. A more meaningful type is only determined for the variable at runtime.


Eg. 
Arrays in Objective-C are not typed. Every element in the array has the type of `id`. So elements of different types can be together in an array.

```objectivec
Person *suyash = [[Person alloc] init];
Person *watermelon = [[Fruit alloc] init];
NSArray *bag = @[suyash, watermelon, @"A String"];

// Generate a random index
NSUInteger randomNumber = arc4random_uniform(3);

// Pick a random object
id mysteryItem = [bag objectAtIndex:randomNumber]

//Print the class of the object
NSLog(@"%@", [mysteryItem class]);
```

What if a method was called on this randomly picked object?

In Swift, the object and the methods calling them are bound during compile time. This does not happen in Objective-C until runtime.

Say, all the three - `suyash`, `watermelon` and `@"A String"` implement a method size.

Adding the following line to the code snippet will give no errors even though it is not known at compile time which implementation of size is to be invoked.

```objectivec
NSLog("%@", [mysteryItem size]);
```
The problem is when one of the elements does not implement the size method and is randomly chosen. THis will return an error saying `Unrecognised selector sent to instance`. A selector is another name for method. 

To defend your code, use the following:

```objectivec
if ([mysteryItem respondsToSelector:@selector(size)]) {
    NSLog("%@", [mysteryItem size]);
}
```

## Objective-C Syntax

Every class is associated with two files:

- Header file (.h): Holds the public interface.
- Implementation file (.m): Holds the method implementations and private properties.

**HEADER FILE EXAMPLE**
```objectivec
#import <Foundation/Foundation.h>

@interface House : NSObject

@property (nonatomic) NSString *address;
@property (nonatomic) int numberOfBedrooms;
@property (nonatomic) BOOL hasBathTub;

@end
```

`Nonatomic` is a property attribute. Properties can either be atomic or nonatomic. This defines how the properties are handled in multithreading. Properties are atomic by default. 

Other property attributes are `readonly`, `readwrite` and `copy`.

If I wanted to allow mutating the `numberOfBedrooms` property only from whithin the class, I add/replace the following lines:

```objectivec
// In House.h
@property (nonatomic, readonly) int numberOfBedrooms;

// In House.m
@property (nonatomic, readwrite) int numberOfBedrooms;
```

As for `copy`, its mostly used for `NSString` properties. Consider the following code:

```objectivec
NSMutableString *name = [[NSMutableString alloc] initWithString: @"Suyash"];

Person *suyash = [[Person alloc] initWithName: suyash];
NSLog(@"%@", suyash.name)
// Output: Suyash

[suyash appendString: @"Shekhar"];
NSLog(@"%@", suyash.name)
// Output: Suyash Shekhar
```

To prevent this problem cause due to the object property referencing a variable pointer, use `copy` such as:

```objectivec
@property (nonatomic, copy) NSString *address;
```

**METHOD DECLARATION**
```objectivec
-(returnType)methodName:(parameterType*) parameterName {
    body;
}
```

The `-` tells us that its an instance method

```objectivec=
-(Book)makeBookWithTitle:(NSString*)title
                  author:(NSString*)author
                    year:(int)year {
    // Method Body
}
```


**INITIALISATION**

```objectivec
House *myHouse = [[House alloc] init]
```

**WRITING A CUSTOM INITIALISER**

```objectivec
#import "House.h"

@implementation Person

-(instancetyoe)initWithName:(NSString*)name {
    // Every initialisation method starts by calling init fmor its super class
    self = [super init]
    
    // If initialised correctly
    if(self) {
        // Initialise with a copy of the name
        _name = [name copy];
        _height = 0;
        _weight = 0;
    }
    
    return self
}
@end
```

To make this initialiser available outside the class, we need to declare it in the public interface (Person.h) as well:

```objectivec
-(instancetype)initWithName: (NSString*)name;
```

**ENUMS**
All Enums in Objective-C are of type NSInteger

```objectivec
#import <Foundation/Foundation.h>

typedef NS_ENUM(NSInteger, Direction) {
    North,
    South,
    East,
    West
};

@interface Bedroom : NSObject

@property (nonatomic) BOOL hasPrivateBath;
@property (nonatomic) Direction dirWindowFaces;

@end
```

**STRONG AND WEAK**
`Strong` and `Weak` are property attributes as well. Pointers are `Strong` by default.

`Strong` references are used when teh property is the primary responsibility of the class i.e is owned by the class. For example, the `frontBedroom` property is the primary responsibility of the House class.

`Weak` references are used for properties that are teh primary responsibility of some other class.

One should use `weak` when:

- Property is a delegate
- Property is a subview of the main view

This is important to avoid memory cycles.