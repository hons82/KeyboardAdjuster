# KeyboardAdjuster

[![CI Status](http://img.shields.io/travis/lionheart/KeyboardAdjuster.svg?style=flat)](https://travis-ci.org/lionheart/KeyboardAdjuster)
[![Version](https://img.shields.io/cocoapods/v/KeyboardAdjuster.svg?style=flat)](http://cocoapods.org/pods/KeyboardAdjuster)
[![Platform](https://img.shields.io/cocoapods/p/KeyboardAdjuster.svg?style=flat)](http://cocoapods.org/pods/KeyboardAdjuster)
![Swift](http://img.shields.io/badge/swift-4-blue.svg?style=flat)

KeyboardAdjuster provides a drop-in `UILayoutGuide` that helps you adjust your views to avoid the keyboard. That's pretty much all there is to it. It's battle-tested and easy to integrate into any project--Storyboards or code, doesn't matter.

KeyboardAdjuster started as a Swift port of [LHSKeyboardAdjuster](https://github.com/lionheart/LHSKeyboardAdjusting), which is recommended for projects written in Objective-C.

KeyboardAdjuster uses a `UILayoutGuide` approach that was heavily inspired by an implementation in [Swiftilities](https://github.com/Raizlabs/Swiftilities/tree/develop/Pod/Classes/Keyboard), by the fine folks at [RaizLabs](http://raizlabs.com).

### Requirements

* [x] Auto Layout
* [x] iOS 9.0-11.2+

### Support KeyboardAdjuster

Supporting KeyboardAdjuster, keeping it up to date with the latest iOS versions, etc., takes a lot of time! So, if you're a developer who's gotten some utility out of this library, please support it by starring the repo. This increases its visibility in GitHub search and encourages others to contribute. 🙏🏻🍻

## Installation

KeyboardAdjuster is available through [CocoaPods](http://cocoapods.org). To install it, simply add the following line to your Podfile:

```ruby
pod "KeyboardAdjuster", "~> 3"
```

## Usage

1. In your view controller file, import `KeyboardAdjuster`.

   ```swift
   import KeyboardAdjuster
   ```

2. Figure out which view you'd like to pin to the top of the keyboard--it's probably going to be a `UIScrollView`, `UITableView`, or `UITextView`. Then, wherever you're setting up your view constraints, use the `keyboardLayoutGuide` property to create a `greaterThanOrEqualTo` constraint to the bottom of the view you'd like to resize:

   ```swift
   class MyViewController: UIViewController {
       func viewDidLoad() {
           super.viewDidLoad()

           // ...
           // Your Auto Layout code here
           // ...

           tableView.bottomAnchor.constraint(lessThanOrEqualTo: view.bottomAnchor).isActive = true
           tableView.bottomAnchor.constraint(greaterThanOrEqualTo: keyboardLayoutGuide.topAnchor).isActive = true
       }
   }
   ```

   <details>
     <summary><strong>NOTE:</strong> If you're using iOS 11 and your view is using the <code>safeAreaLayoutGuide</code> to set constraints, click here to view an alternate approach.</summary>

     ```swift
     func viewDidLoad() {
         super.viewDidLoad()

         if #available(iOS 11, *) {
             tableView.bottomAnchor.constraint(lessThanOrEqualTo: view.safeAreaLayoutGuide.bottomAnchor).isActive = true
         } else {
             tableView.bottomAnchor.constraint(lessThanOrEqualTo: view.bottomAnchor).isActive = true
         }

         tableView.bottomAnchor.constraint(greaterThanOrEqualTo: keyboardLayoutGuide.topAnchor).isActive = true
     }
     ```
   </details>

3. And you're done! Whenever a keyboard appears, your view will be automatically resized.

## Optional Features

KeyboardAdjuster also allows you to provide callbacks when the keyboard state changes or specify whether to animate the transition (animated by default). If you'd like to take advantage of these, just make your `UIViewController` conform to `KeyboardAdjusterOptions`, like so:


```swift
class MyViewController: UIViewController, KeyboardAdjusterOptions {
    var animateKeyboardTransition = true

    func keyboardWillHideHandler() {
        print("Hiding keyboard...")
    }

    func keyboardWillShowHandler() {
        print("Showing keyboard...")
    }
}
```

## How It Works

KeyboardAdjuster registers NSNotificationCenter callbacks for keyboard appearance and disappearance. When a keyboard appears, it pulls out the keyboard size from the notification, along with the duration of the keyboard animation, and applies that to the `keyboardLayoutGuide` property.

## Author

[Dan Loewenherz](https://github.com/dlo)


## License

KeyboardAdjuster is available under the Apache 2.0 LICENSE. See the [LICENSE](LICENSE) file for more info.
