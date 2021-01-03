# libLottie for jailbroken iOS

## Introduction

Lottie is a mobile library for Android and iOS that parses [Adobe After Effects](http://www.adobe.com/products/aftereffects.html) animations exported as json with [bodymovin](https://github.com/bodymovin/bodymovin) and renders the vector animations natively on mobile and through React Native!

## libLottie

libLottie is a library for jailbroken iOS that helps you to run Lottie animations in your tweaks, Supports iOS11+


## Install Instructions

* Place `libLottie.dylib` in the `$THEOS/lib` directory
* Place `Lottie` folder in the `$THEOS/include` directory

## Usage Instructions

* Add `XXX_LIBRARIES += Lottie` to you Makefile.
* Then add `Depends: com.miro.liblottie` to your control file
* In your source files, e.g. Tweak.xm `#import <Lottie/Lottie.h>`
* This library is currently hosted on my personal repo `https://miro92.com/repo`


## Lottie Usage Examples

Lottie animations can be loaded from bundled JSON or from a URL

```objective-c
LOTAnimationView *animation = [LOTAnimationView animationNamed:@"Lottie"];
[self.view addSubview:animation];
[animation playWithCompletion:^(BOOL animationFinished) {
  // Do Something
}];

// or

LOTAnimationView *animation = [LOTAnimationView animationWithFilePath:@"/Library/Application Support/MYTweak/Resources/test.json"];
[self addSubview:animation];

```

If you are working with multiple bundles you can use.

```objective-c
LOTAnimationView *animation = [LOTAnimationView animationNamed:@"Lottie" inBundle:[NSBundle YOUR_BUNDLE]];
[self.view addSubview:animation];
[animation playWithCompletion:^(BOOL animationFinished) {
  // Do Something
}];
```

Or you can load it programmatically from a NSURL
```objective-c
LOTAnimationView *animation = [[LOTAnimationView alloc] initWithContentsOfURL:[NSURL URLWithString:URL]];
[self.view addSubview:animation];
```

Lottie supports the iOS `UIViewContentModes` aspectFit, aspectFill and scaleFill

You can also set the animation progress interactively.
```objective-c
CGPoint translation = [gesture getTranslationInView:self.view];
CGFloat progress = translation.y / self.view.bounds.size.height;
animationView.animationProgress = progress;
```

Or you can play just a portion of the animation:
```objective-c
[lottieAnimation playFromProgress:0.25 toProgress:0.5 withCompletion:^(BOOL animationFinished) {
  // Do Something
}];
```
## iOS View Controller Transitioning

Lottie comes with a `UIViewController` animation-controller for making custom viewController transitions!
Just become the delegate for a transition

```objective-c
- (void)_showTransitionA {
  ToAnimationViewController *vc = [[ToAnimationViewController alloc] init];
  vc.transitioningDelegate = self;
  [self presentViewController:vc animated:YES completion:NULL];
}
```

And implement the delegate methods with a `LOTAnimationTransitionController`

```objective-c
#pragma mark -- View Controller Transitioning

- (id<UIViewControllerAnimatedTransitioning>)animationControllerForPresentedController:(UIViewController *)presented presentingController:(UIViewController *)presenting sourceController:(UIViewController *)source {
  LOTAnimationTransitionController *animationController = [[LOTAnimationTransitionController alloc] initWithAnimationNamed:@"vcTransition1" fromLayerNamed:@"outLayer" toLayerNamed:@"inLayer" applyAnimationTransform:NO];
  return animationController;
}

- (id<UIViewControllerAnimatedTransitioning>)animationControllerForDismissedController:(UIViewController *)dismissed {
  LOTAnimationTransitionController *animationController = [[LOTAnimationTransitionController alloc] initWithAnimationNamed:@"vcTransition2" fromLayerNamed:@"outLayer" toLayerNamed:@"inLayer" applyAnimationTransform:NO];
  return animationController;
}

```

By setting `applyAnimationTransform` to YES you can make the Lottie animation move the from and to view controllers. They will be positioned at the origin of the layer. When set to NO Lottie just masks the view controller with the specified layer while respecting z order.


## More
For more info please check out the official [lottie-ios](https://github.com/airbnb/lottie-ios) fork.
