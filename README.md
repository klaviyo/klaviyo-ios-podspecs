# Klaviyo iOS CocoaPods specs

Prototype of a Klaviyo-owned public CocoaPods spec repository. Initially contains
the five pods from the published Swift SDK 5.5.0 release. Older releases have not
been backfilled. SDK source remains in the tagged Swift SDK repository.

## Install

Add these sources at the top of your Podfile, in this order:

```ruby
source 'https://github.com/klaviyo/klaviyo-ios-podspecs.git'
source 'https://cdn.cocoapods.org/'
```

Keep your existing Klaviyo pod declarations, then run `pod install --repo-update`.
For the main SDK, explicitly select the spec source when the same version also
exists on trunk:

```ruby
pod 'KlaviyoSwift', '5.5.0', :source => 'https://github.com/klaviyo/klaviyo-ios-podspecs.git'
```

To upgrade a previously locked SDK, use `pod update` with the Klaviyo pod names
you installed, including the transitive KlaviyoCore pod. Review and commit the
resulting Podfile.lock.

The public CDN supplies third-party dependencies such as AnyCodable-FlightSchool.
The Klaviyo source must come first because CocoaPods uses the first source that
contains a pod. Keep existing additional sources required by your application.
Check the `SPEC REPOS` section in Podfile.lock to confirm that all Klaviyo pods
resolve from this repository.

Swift Package Manager remains available through
[klaviyo-swift-sdk](https://github.com/klaviyo/klaviyo-swift-sdk).

## Publish a release manually

This prototype does not change the Swift SDK's existing trunk publishing workflow.
Publishing here requires write access to this repository and CocoaPods installed.
Only publish podspecs from an existing public SDK release tag; never replace an
already published version with different contents.

Register the repository once:

```sh
pod repo add klaviyo https://github.com/klaviyo/klaviyo-ios-podspecs.git
```

From a clean Swift SDK checkout at the release tag, publish in dependency order:

```sh
pod repo push klaviyo KlaviyoCore.podspec --allow-warnings --no-overwrite
pod repo push klaviyo KlaviyoSwiftExtension.podspec --allow-warnings --no-overwrite
pod repo push klaviyo KlaviyoSwift.podspec --allow-warnings --no-overwrite
pod repo push klaviyo KlaviyoForms.podspec --allow-warnings --no-overwrite
pod repo push klaviyo KlaviyoLocation.podspec --allow-warnings --no-overwrite
```

`pod repo push` validates each spec before pushing. Consumers must update their
spec repository to see newly published versions. This repository contains only
spec metadata; preserve the corresponding SDK Git tags and source availability.

CocoaPods may warn about duplicate specs while the same release exists here and
on trunk. Inspect Podfile.lock to verify the selected source instead of assuming
that a successful installation used this repository.

Before adopting this as the supported release channel, automate publishing,
define historical version coverage, validate lockfile upgrades, and document
ownership of CocoaPods and Xcode compatibility.
