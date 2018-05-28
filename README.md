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
| -------- | -------- | -------- |
 Swift uses Optionals and only they can be `nil`    | Any object can have a `nil` value.     |

Objective-C has no `NullPointerException` or any such consequence of using a nil pointer. 

Eg. 
```objectivec
Person *suyash = nil;
NSLog(@"My name is %@", suyash);

// Output: (null)
```

- **Mutability**

 Swift | Objective-C |
| -------- | -------- | -------- |
 Constants are constant, variables vary, and by default we are encouraged to favor constants    | All object attributes are mutable by default     |

Objective-C classes with limited mutability include all Foundation collections. For example, `NSStrings` are not mutable; to make a change to a string one must either make a new string or use `NSMutableString`. The same goes for `NSArray` and `NSMutableArray`, `NSDictionary` and `NSMutableDictionary`, `NSSet` and `NSMutableSet`, and the remaining classes which you can read about here.

Swift favours use of value types like structs while Objective-C favours reference types like classes.
