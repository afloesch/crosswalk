## Introduction

Crosswalk is an app runtime based on Chromium/Blink.

It is an open source project started by the Intel Open Source Technology Center
(http://www.01.org)

This version has been forked from https://github.com/crosswalk-project/crosswalk, to provide a custom Android build which relaxes the chromium same-origin policy, to allow for access from documents with the same domain, and relaxes the document Content Security Policy settings to allow scripts loaded from lifelock.com.

These changes to Chromium are essential to the Identity app javascript engine, so that it can access frames and execute operations in child frames without running into security restrictions, much like a content script in a chrome extension.

Rather than completely disable the security settings, we simply relax the rules slightly to allow for the Identity app use-case.

## Getting started

Most of these instructions have come from the [Crosswalk Android build instructions](https://crosswalk-project.org/contribute/building_crosswalk/android_build.html). If you run into trouble with these steps go there.

### Install Chromium depot_tools

```sh
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
export PATH="$PATH:/path/to/depot_tools"
```

### Fetch Source

```sh
export XWALK_OS_ANDROID=1
gclient config --name src/xwalk https://github.com/afloesch/crosswalk.git
```

Open the .gclient file that was created and add the following text to the bottom of the file.

```
target_os = ['android']
```

Fetch the latest Chromium source. This will take a while...

```sh
gclient sync
```

Install Android dependencies.

```sh
cd src
build/install-build-deps-android.sh
```

Generate build target settings.

```sh
gn args out/Default
```

This will open a text file. Add the following lines to the file and save it.

```sh
import("//xwalk/build/android.gni")
target_os = "android"
```

### Build Crosswalk

```sh
ninja -C out/Default xwalk_core_library
```

## Documents

Check out the Crosswalk [wiki](http://crosswalk-project.org/#wiki).

## Community

How to use Crosswalk you can ask on the mailing list: https://lists.crosswalk-project.org/mailman/listinfo/crosswalk-help

Development of Crosswalk: https://lists.crosswalk-project.org/mailman/listinfo/crosswalk-dev

We are also on IRC: `#crosswalk` @ `chat.freenode.net`

## License

Crosswalk's code uses the 3-clause BSD license, see our `LICENSE` file.
