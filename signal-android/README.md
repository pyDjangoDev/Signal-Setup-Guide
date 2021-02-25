# Signal Android
This guide is written using Signal-Android version 5.3.12

## Requirement
* Android Studio 4.0
* Java SE 14
* SDK 28
* NDK

## Installation Steps
1. First clone the project source code:

```
git clone https://github.com/signalapp/Signal-Android.git && cd Signal-Android
```

2. Comment out `distributionSha256Sum` on `gradle/wrapper/gradle-wrapper.properties` and change the grale version from `5.6.2` to `6.5.1`

```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
#distributionSha256Sum=027fdd265d277bae65a0d349b6b8da02135b0b8e14ba891e26281fa877fe37a2
distributionUrl=https\://services.gradle.org/distributions/gradle-6.4.1-all.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists
```

3. Install the `21.0.6113669` NDK version [link](https://developer.android.com/studio/projects/install-ndk)
4. Use 'Keystore Explorer’, edit `whisper.store` files (the password is "whisper" without quote), insert your all your server certificates there (pem format). 
5. Update URL with own server in `app/build.gradle` (be sure to use https and don't include trailing slash). If you are having a hard time finding your `Cloudfront domain`, you can find it in <a href="https://console.aws.amazon.com/cloudfront/">CloudFront console</a> formated as `random-id.cloudfront.net`.
```
...

defaultConfig {
        versionCode canonicalVersionCode * postFixSize
        versionName canonicalVersionName

        minSdkVersion 19
        targetSdkVersion 28
        multiDexEnabled true

        vectorDrawables.useSupportLibrary = true
        project.ext.set("archivesBaseName", "Signal");

        buildConfigField "long", "BUILD_TIMESTAMP", getLastCommitTimestamp() + "L"
        buildConfigField "String", "SIGNAL_URL", "\"https://domain.com\""
        buildConfigField "String", "STORAGE_URL", "\"https://cloudfrontdomain.com\""
        buildConfigField "String", "SIGNAL_CDN_URL", "\"https://cloudfrontdomain.com\""
        buildConfigField "String", "SIGNAL_CONTACT_DISCOVERY_URL", "\"https://domain.com\""
        buildConfigField "String", "SIGNAL_SERVICE_STATUS_URL", "\"https://domain.com\""
        buildConfigField "String", "SIGNAL_KEY_BACKUP_URL", "\"https://domain.com\""
        buildConfigField "String", "CONTENT_PROXY_HOST", "\"https://domain.com\""
        buildConfigField "int", "CONTENT_PROXY_PORT", "443"
        buildConfigField "String", "USER_AGENT", "\"OWA\""
        buildConfigField "boolean", "DEV_BUILD", "false"
        buildConfigField "String", "MRENCLAVE", "\"cd6cfc342937b23b1bdd3bbf9721aa5615ac9ff50a75c5527d441cd3276826c9\""
        buildConfigField "String", "KEY_BACKUP_ENCLAVE_NAME", "\"f2e2a5004794a6c1bac5c4949eadbc243dd02e02d1a93f10fe24584fb70815d8\""
        buildConfigField "String", "KEY_BACKUP_MRENCLAVE", "\"f51f435802ada769e67aaf5744372bb7e7d519eecf996d335eb5b46b872b5789\""
        buildConfigField "String", "UNIDENTIFIED_SENDER_TRUST_ROOT", "\"public-key-generated-in-signal-server-step-3\""
        buildConfigField "String[]", "LANGUAGES", "new String[]{\"" + autoResConfig().collect { s -> s.replace('-r', '_') }.join('", "') + '"}'
        buildConfigField "int", "CANONICAL_VERSION_CODE", "$canonicalVersionCode"

...
```

5. update `UNIDENTIFIED_SENDER_TRUST_ROOT` (value is Public Key from when creating UnidentifiedDelivery of server’s config.yml)
6. Download `google-services.json` from Firebase, put it inside `app/`.
7. Update `app/src/main/res/values/firebase_messaging.xml` according to value from `google-services.json`
8. Update `ATTACHMENT_DOWNLOAD_PATH` and `ATTACHMENT_UPLOAD_PATH` in `libsignal/service/src/main/java/org/whispersystems/signalservice/internal/push/PushServiceSocket.java` by deleting ‘attachments/‘ so attachment will be uploaded in root ( / ). If you don't want the attachments to be uploaded to root bucket, check the FAQ part of this guide.
9. Sync your project then build.

