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
[RACObserve(self, something) subscribeNext: ^(id const something)
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

## Check invariants with NSAssert macros
When your methods have required parameters you should always check their presence via `NSParameterAssert(...);` macro. These checks should be the first thing you do in a method body.

_**Hint**: for C functions use an `NSCParameterAssert(...)` macro._

When you write code you make numerous assumptions about the state of a surrounding system. Sometimes your assumptions are wrong. It's a good practice to fix your assumptions in code in a way of `NSAsserts` so you can get meaningful exceptions when your code breaks.

## Canonical view controller implementation file structure

```objective-c
#pragma mark - Initialization

// Various initializers go here.

#pragma mark - Cleanup

// The sole -dealloc method here.

#pragma mark - SuperClass Overrides

// Superclass method overrides go here.

#pragma mark - Reactivity

// Bindings and declarative logic of a ReactiveCocoa goes here.

#pragma mark -

// The sole -awakeFromNib goes here.

#pragma mark - Interface Callbacks

// IBActions from UI go here.

#pragma mark - Lazy Initialization

// Getters that perform a lazy objects initialization go here.

#pragma mark - Public Methods

// Public methods go here.

#pragma mark - Private Methods

// Private methods go here. Class can have a numerous 'Private Methods' sections, in which case they should be named like 'Private Methods | Group Name' and so on...

#pragma mark - InformalProtocol Informal Protocol Implementation

// Informal protocol implementation goes here.

#pragma mark - SomeProtocol Protocol Implementation

// Protocol implementation goes here.

#pragma mark - Localization

// Sometimes there is a need to inject localized strings in a dynamic way.
```

## All views should be laid out via Auto Layout
Since the Auto Layout introduction in OS X Lion (in 2011) I have not written a single -setFrame: call and never set an autoresizing mask. And so should you.

When you initialize a view in code, pass `NSZeroRect` as a parameter to `-initWithFrame:` method:

```objective-c
NSView* const view = [[NSView alloc] initWithFrame: NSZeroRect];`
```

## Class files: public header, class extension and implementation
For a class called `MyClass` these files should be named correspondingly: `MyClass.h`, `MyClass+Private.h` and `MyClass.m`.

Public header should contain only public API of the class. Every implementation detail that needs to be declared in a header should go to a class extension (`readwrite` property redefines, private methods declarations that are subject for override in a subclass and so on...). When making a subclass remember that you need to import a class extension (`MyClass+Private.h`), not a public header.

## Poor man's optionals: `objectOrNil`
Untill the recent introduction of a nullability annotations it always was non-obvious whether you can get a nil instead of a meaningful object from some method (after looking at its signature in a header). The only way determine it was to read the docs.

There is a prevailing opinion that `nils` are dangerous. They were even considered a billion dollar mistake.

Until nullability annotations were added to the Objective-C I had a naming convention that allowed me not to neglect the fact I can get a nil reference at some point.

```objective-c
id objectOrNil = <...>

if(objectOrNil)
{
  // Got it!
}
else
{
  // Error!
}
```

Code is written once, but read and edited multiple times, possibly even by someone of your co-workers. When you write it in a first place you check the docs and examine all possible return values from a method. When time passes and you return to a code to make improvements it is very easy to miss the important details.

Even with nullability annotations it is still a good idea to follow this naming convention. Some may say it is too verbose and to some extent resembles an ugly hungarian notation, but I prefer explicit and obvious when it comes to dealing with nils.
