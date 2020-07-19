# Signal iOS
This guide is written using Signal-iOS version 3.11.0.24

## Requirement

* Latest stable version of Xcode.

## Installation Steps

1. Clone

Clone the repo to a working directory:

```
git clone --recurse-submodules https://github.com/signalapp/Signal-iOS
```

Since we make use of sub-modules, you must use `git clone`, rather than
downloading a prepared zip file from Github.

We recommend you fork the repo on GitHub, then clone your fork:

```
git clone --recurse-submodules https://github.com/<USERNAME>/Signal-iOS.git
```

You can then add the Signal repo to sync with upstream changes:

```
git remote add upstream https://github.com/signalapp/Signal-iOS
```

2. Dependencies

To build and configure the libraries Signal uses, just run:

```
make dependencies
```

### Building RingRTC

A prebuilt version of WebRTC.framework and the libringrtc static library reside
in a sub-module and should be installed by the above steps.  However, if you'd 
like to build it from source, see: https://github.com/signalapp/ringrtc

3. Xcode

Open the `Signal.xcworkspace` in Xcode.

```
open Signal.xcworkspace
```

In the TARGETS area of the General tab, change the Team drop down to
your own. You will need to do that for all the listed targets, for ex. 
Signal, SignalShareExtension, and SignalMessaging. You will need an Apple
Developer account for this. 

On the Capabilities tab, turn off Push Notifications and Data Protection,
while keeping Background Modes on. The App Groups capability will need to
remain on in order to access the shared data storage. The App ID needs to
match the SignalApplicationGroup string set in TSConstants.h. 

If you wish to test the Documents API, the iCloud capability will need to
be on with the iCloud Documents option selected.

Build and Run and you are ready to go!