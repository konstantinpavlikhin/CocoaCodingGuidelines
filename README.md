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
