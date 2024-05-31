---
layout: post
title:  "[iOS] When Developing Swift Package"
date:   2024-05-30 21:10:00 -0500
categories: iOS SPM tip
---
## 1. To edit Swift Package you are developing, from the app bundle using this package
There is a WWDC video about this need.\
[https://developer.apple.com/videos/play/wwdc2019/410/](https://developer.apple.com/videos/play/wwdc2019/410/)

You can drag-drop the folder where `Package.swift` is rooted. After Project already added the package.

Something I noticed from the video is that, firstly, regular Package Dependencies adding was done already, and without anything else, like reverting it, immediately drag-and-drop into Xcode bundle happened, keeping its target intact, and re-bundle the package to be editable. Until I noticed this, I had hard time replicating what the WWDC video was doing


## 2. If the package you are developing has another package of yours as dependency
You may be developing multiple packages, and they are dependent to one another, as described in `Package.swift` like:

```swift
dependencies: [
    // Dependencies declare other packages that this package depends on.
    .package(url: "https://github.com/{owner}/{package}", branch: "master"),
],
```
that is using remote origin repository URL. 

However, if all these packages are locally bundled to be edited, this kind of dependency to remote origin will cause build error like:
```shell
error: ~/Library/Developer/Xcode/DerivedData/{app}-{}/Build/Intermediates.noindex/{app}.build/Debug-iphonesimulator/{app}.build/Objects-normal/arm64/{app}.swiftmodule: No such file or directory (in target {app} from project {target})
```
and I am assuming, because dependency is locally bundled as well, we need to (temporarily) change dependency `url` like:
```swift
.package(url: "~/{localPath}/{package}/", branch: "master"),
```
because we are bundling the package from local path, not from the remote origin repository url.