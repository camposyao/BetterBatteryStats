*Build* [![CircleCI](https://circleci.com/gh/asksven/BetterBatteryStats/tree/master.svg?style=svg)](https://circleci.com/gh/asksven/BetterBatteryStats/tree/master)

#License
BetterBatteryStats is an open source project unter the terms of the Apache 2.0 License. The license does not apply to the use of the names "BetterBatteryStats" and "Better Battery Stats", nor to the icon / artwork created for BetterBatteryStats. 

# Build
In order to build (with gradle / Android Studio) following changes to the local project are required

## google-services.jon
BBS uses Firebase Analytics. You will need to create a `google-services.json` in (follow the Firebase instructions):
- `/app`
- `/app/src/xdaedition`
- `/app/src/gplay`

## gradle.properties

`MyProject.signing` points to a private file (`asksven.graddle`) where the `SigningConfigs` are defined.

The config points to two keystores:
- release: `asksven.keystore`
- debug: `asksven_debug.keystore`

Create a private file `asksven.gradle` with following content (or whatever name you want) and change `MyProject.signing` accordingly:

```
android {
  signingConfigs {
    release {
      storeFile file(project.property("MyProject.signing") + ".keystore")
      storePassword "<your-store-password-here>"
      keyAlias "<your-key-alias-here>"
      keyPassword "<your-key-password-here>"
    }
    debug {
      storefile file(project.property("MyProject.signing") + "_debug.keystore")
  }

  buildTypes {
    release {
      signingConfig signingConfigs.release
    }
    debug {
      signingConfig signingConfigs.debug
  }
}
```

# Continuous Integration

The continuous integration (in this example CircleCI) needs to have access to some private settings.

## google-services.json

The encrypted files (`google-services.json-cipher`) are located in `/app`, `/app/src/gplay` and `/app/src/xdaedition`.

See also https://github.com/circleci/encrypted-files.

### Encrypt

`openssl aes-256-cbc -e -in secret-env-plain -out secret-env-cipher -k $KEY`

### Decrypt (on CircleCI, as defined in `circle.yml` and using an env-variable `KEY`)

`openssl aes-256-cbc -d -in secret-file-cipher -out secret-file-plain -k $KEY`
