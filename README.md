## Dot syntax vs square brackets
Objects in Objective-C have properties and methods. Properties should be accessed via dot syntax and methods should be called via square brackets.

_**Note**: with every OS X release Apple updates API of its frameworks. Many things that were previously declared as a setter and getter methods gradually transition to a property declarations. Your source code should reflect these changes as they happen._

## Symbols prefixes
All classes, functions, enumerations and global constants (numbers, strings, notifications and so on...) should be prefixed with a three letters.

_**Hint**: for personal projects and reusable components consider using abbreviation of your name, middle name and a last name. When working for a client stick with abbreviation of a project name. Just make sure you pick one that sounds good._

## Use Interface Builder when making UI
Views created and configured in code are understandable only to their authors. And only within the first two weeks after a commit. Utilize Interface Builder. Finally, the Auto Layout support is so good you no longer have to fight with the tool.

## Hierarchical structure of a project files
For every separate object (class) in a project an Xcode group (yellow one) should be created. These groups should be named after the corresponding objects minus the mandatory three-letter prefix.

```
┌──────────────────────────┐                      
│ DuplicatesViewController │                      
└───┬──────────────────────┘                      
    │                                            
    ├── SGMDuplicatesViewControllerDelegate.h     
    │                                            
    ├── SGMDuplicatesView.xib                     
    │                                            
    ├── SGMDuplicatesViewController.h             
    │                                            
    ├── SGMDuplicatesViewController+Private.h     
    │                                            
    ├── SGMDuplicatesViewController.m             
    │                                            
    ├── SGMDuplicatesViewController.strings       
    │                                            
    ├── SGMDuplicatesViewController.stringsdict   
    │                                            
    └── SGMDuplicatesViewController.xcassets
```

## Nib files naming convention
On OS X you generally work with two types of nibs: view nibs and window nibs.

* Imagine you have a view controller named `ASDMessagesViewController`. Its corresponding view nib should be named `ASDMessagesView`, dropping the 'Controller'
 part.
* Imagine you have a window controller named `ASDMainWindowController`. Its corresponding window nib should be named `ASDMainWindow`, dropping the 'Controller'
 part.

## Prefer an old-school nibs to storyboards
When building a complex application stick with an old-fashioned nib-based approach for building UI. It's much better to keep things separated and manageable than to put everything in one place.

## Take advantage of a Base Internationalization feature
When you need to localize static UI strings prefer to use Base Internationalization feature. Under no circumstances should you multiply the nib-files under different lproj-subdirectories! Base Internationalization approach is DRY. It also liberates you from the need to invent custom localized strings keys and makes manual strings injection unnecessary.

## Extensive usage of a `const` qualifier
Every 'variable' that is not intended to be altered down the control flow should be declared as constant. It is much easier to reason about the algorithm when there are things that doesn't change under your very nose.

```objective-c
// Magic numbers should be const.
const NSUInteger magicNumber = 42;

// Notifications should be const.
NSString* const KSPUsefulNotification = @"KSPUsefulNotification";

// Any objects handles should be const.
NSImage* const image = [NSImage imageNamed: @"NSActionTemplate"];

// Intermediate state calculations should be const.
const BOOL canMarkChatAsUnread = (clickedChatOrNil && [self canMarkChatAsUnread: clickedChatOrNil]);

// Parameters should be const.
[RACObserve(self, something) subscribeNext: ^(id* const something)
{
	// ...
}];
```

Const all the things!

## Take advantage of a static typing
Everything that may be typed should be typed.

For every `NSViewController` subclass that you have you should re-declare its `representedObject` property type from `id` to some meaningful represented object class. Don't forget to add a `@dynamic representedObject;` to the implementation file.

For every `NSTableCellView` subclass that you have you should re-declare its `objectValue` property type from `id` to some meaningful object value class. Don't forget to add a `@dynamic objectValue;` to the implementation file.

## Property definitions should be explicit
All property definitions should include all possible property attributes.

```objective-c
@property(readwrite, strong, nonatomic) ClassName* propertyName;
```

_**Note**: a new `nullable` and `nonnull` attributes should be added (to the end of the list) where it makes sense. See the `NS_ASSUME_NONNULL_BEGIN/NS_ASSUME_NONNULL_END` macro._

Explicit is much better than implicit when it comes to property semantics.
