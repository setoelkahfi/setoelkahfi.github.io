---
layout: post
title: "[SOLVED] Could not find firebase-iid-interop.aar (com.google.firebase:firebase-iid-interop:16.0.0)."
date: 2018-11-30 11:59:41 +0100
comments: true
categories: [Technical, Android Development]
---
In this my first work in Sweden, I was applying for iOS developer role. With my strong selling points were I've been developing iOS application for almost 3 years, showed my then potential employer all the projects that I've involved in (3 iOS apps, 3 websites, and I mentioned Android app, but couldn't showed it to them because I'm using iPhone now), and my preference is Swift other than Objective-C. But, in this second week of my work, I've already touch our Android codebase, take a look at our web admin project which is written in AngularJS, and our iOS codebase is written in Objective-C. I couldn't excited more :).

Now, let's back to what I'm going to write.
<!-- more -->

<h2>Android Project With Multiple Build Variants</h2>

When I was setting up the Android project, I face this simple issue. I said simple because it is. Our Android project has multiple build variants. Meaning, it has different build flavours. Each client has its own build flavor. And each build type defines environment of the app: which back end it communicates to. Oh, this is for our old app system, which is in the same project for our upcoming new app system, but in different branch. 

So, here's the error message when I try to build the project in Android Studio:

``` bash
Could not find firebase-iid-interop.aar (com.google.firebase:firebase-iid-interop:16.0.0).
Searched in the following locations:
    https://jcenter.bintray.com/com/google/firebase/firebase-iid-interop/16.0.0/firebase-iid-interop-16.0.0.aar
```

This is happened because Google removed their projects from `jcenter`, but erroneously left some artifacts with dependencies [1]. 

<h2>Simple Solution</h2>
You need to put `google()` repository BEFORE `jcenter()` [1] like so:
``` bash
allprojects {
    repositories {
        jcenter()
        maven {
            url "https://maven.google.com"
        }
        google()
    }
}
```

And because each build variant has its own gradle file, I need to edit the root gradle file. Also, you need to pay attention to which repositories section you need to edit. Make sure its `allprojects.repositories` section, not the `buildscript.repositories` section [2].

And that's it. I tried to clean up the project, and then rebuild again, and viola! It works.

[1] https://stackoverflow.com/a/53158506/1137814</br>
[2] https://github.com/geektimecoil/react-native-onesignal/issues/552#issuecomment-433077966