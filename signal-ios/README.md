# Signal iOS
This guide is written using Signal-iOS version 3.19.0

## Requirement

* Latest stable version of Xcode.

## Installation Steps

1. Clone

Clone the repo to a working directory:

```
git clone --recurse-submodules https://github.com/signalapp/Signal-iOS
```

2. Dependencies

To build and configure the libraries Signal uses, just run:

```
make dependencies
```

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

## Push Notifications
In order to enable the push notifications you must generate a push certificate/key(Voip type), and convert them using this commands:

```
openssl pkcs12 -in certificate.p12 -nokeys -out certificate.cert.pem -nodes

openssl pkcs12 -in certificate.p12 -nocerts -out certificate.key.pem -nodes
openssl rsa -in certificate.key.pem -out certificate.key.pem -text
```

