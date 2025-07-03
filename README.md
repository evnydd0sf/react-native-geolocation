# react-native-geolocation [![npm](https://img.shields.io/npm/v/@react-native-community/geolocation)](https://www.npmjs.com/package/@react-native-community/geolocation) ![Supports Androi* `skipPermissionRequests` (boolean) - Defaults to `false`. If `true`, you must request permissions before using Geolocation APIs.
* `locationProvider` (string, Android-only) - Either `"playServices"`, `"android"`, or `"auto"`.  Determines wether to use `Google's Location Services API` or `Android's Location API`. The `"auto"` mode defaults to `android`, and falls back to Android's Location API if play services aren't available.

**已移除的 iOS 选项：**
* `authorizationLevel` - iOS 专用选项，已随 iOS 支持一起移除
* `enableBackgroundLocationUpdates` - iOS 专用选项，已随 iOS 支持一起移除nd web](https://img.shields.io/badge/platforms-android%20%7C%20web-lightgrey.svg) ![MIT License](https://img.shields.io/npm/l/@react-native-community/geolocation.svg)

> 这是 [@react-native-community/geolocation](https://github.com/michalchudziak/react-native-geolocation) 的 fork 版本，专为解决 Apple Clip App 兼容性问题而创建。

## 修改内容

- **移除了 iOS 相关功能以避免 Apple Clip App 问题**
  - **问题描述**：原版本在处理 Apple Clip App 时会因 `requestAlwaysAuthorization` 导致 Apple Connect 拒绝
  - **根本原因**：iOS 位置服务的权限请求机制与 Apple Clip App 的限制冲突
  - **解决方案**：完全移除 iOS 平台支持，专注于 Android 和 Web 平台
  - **测试状态**：✅ Apple Clip App 兼容 ✅ Android 功能正常 ✅ Web 功能正常

The Geolocation API 📍 module for React Native that extends the [Geolocation web spec](https://developer.mozilla.org/en-US/docs/Web/API/Geolocation). 

Supports TurboModules ⚡️ and legacy React Native architecture.

Fully compatible with TypeScript.

Supports modern [Play Services Location API](https://developers.google.com/android/reference/com/google/android/gms/location/FusedLocationProviderClient.html).


## Supported platforms

| Platform | Support                         |
| -------- | ------------------------------- |
| iOS      | ❌ (已移除以兼容 Apple Clip App) |
| Android  | ✅                               |
| Web      | ✅                               |
| Windows  | ❌                               |
| macOS    | ❌                               |

## Compatibility
| React Native | RNC Geoloaction  |
| ------------ | ---------------- |
| >= 0.73.0    | >= 3.2.0         |
| >= 0.70.0    | >= 3.0.0 < 3.2.0 |
| >= 0.64.0    | 2.x.x            |
| <= 0.63.0    | 1.x.x            |


## Getting started

`yarn add @react-native-community/geolocation`

or

`npm install @react-native-community/geolocation --save`

## Configuration and Permissions

<div class="banner-crna-ejected">
  <h3>Projects with Native Code Only</h3>
  <p>
    This section only applies to projects made with <code>react-native init</code>
    or to those made with <code>expo init</code> or Create React Native App which have since ejected. For
    more information about ejecting, please see
    the <a href="https://github.com/react-community/create-react-native-app/blob/master/EJECTING.md" target="_blank">guide</a> on
    the Create React Native App repository.
  </p>
</div>

### iOS

#### 注意：iOS 支持已被移除

为了兼容 Apple Clip App，此 fork 版本已完全移除 iOS 平台支持。原版本中的 `requestAlwaysAuthorization` 功能会导致 Apple Connect 在处理 Apple Clip App 时拒绝应用。

如果您需要 iOS 支持，请使用原版的 [@react-native-community/geolocation](https://github.com/michalchudziak/react-native-geolocation)。

### Android

To request access to location, you need to add the following line to your app's `AndroidManifest.xml`:

`<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />`

or 

`<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />`

Android API >= 18 Positions will also contain a `mocked` boolean to indicate if position was created from a mock provider.

<p>
  Android API >= 23 Requires an additional step to check for, and request
  the ACCESS_FINE_LOCATION or ACCESS_COARSE_LOCATION permissions using
  the <a href="https://reactnative.dev/docs/permissionsandroid.html" target="_blank">PermissionsAndroid API</a>.
  Failure to do so may result in a hard crash.
</p>

<details>
  <summary><b>For React Native < 0.65 on Android we need to link manually</b></summary>


- android/settings.gradle
```
include ':react-native-community-geolocation'
project(':react-native-community-geolocation').projectDir = new File(rootProject.projectDir, '../node_modules/@react-native-community/geolocation/android')
```
- android/app/build.gradle
```
dependencies {
   ...
   implementation project(':react-native-community-geolocation')
}
```
- android/app/src/main/.../MainApplication.java
  On imports section:
```java
import com.reactnativecommunity.geolocation.GeolocationPackage;
```
  In the class at `getPackages` method: 
```java
@Override
protected List<ReactPackage> getPackages() {
      @SuppressWarnings("UnnecessaryLocalVariable")
      List<ReactPackage> packages = new PackageList(this).getPackages();
      // Packages that cannot be autolinked yet can be added manually here, for example:
      packages.add(new GeolocationPackage()); // <== add this line
      return packages;
}
```
</details>

## Migrating from the core `react-native` module
This module was created when the Geolocation was split out from the core of React Native. As a browser polyfill, this API was available through the `navigator.geolocation` global - you didn't need to import it. To migrate to this module you need to follow the installation instructions above and change following code:

```javascript
navigator.geolocation.setRNConfiguration(config);
```

to:

```javascript
import Geolocation from '@react-native-community/geolocation';

Geolocation.setRNConfiguration(config);
```

If you need to have geolocation API aligned with the browser (cross-platform apps), or want to support backward compatibility, please consider adding following lines at the root level, for example at the top of your App.js file (only for [react native](https://reactnative.dev/docs/platform-specific-code.html#native-specific-extensions-ie-sharing-code-with-nodejs-and-web)):

```javascript
navigator.geolocation = require('@react-native-community/geolocation');
```

## Usage

### Example

```javascript
import Geolocation from '@react-native-community/geolocation';

Geolocation.getCurrentPosition(info => console.log(info));
```

Check out the [example project](example) for more examples.

## Methods

### Summary

* [`setRNConfiguration`](#setrnconfiguration)
* [`requestAuthorization`](#requestauthorization)
* [`getCurrentPosition`](#getcurrentposition)
* [`watchPosition`](#watchposition)
* [`clearWatch`](#clearwatch)
* [`stopObserving`](#stopobserving)

---

### Details

#### `setRNConfiguration()`

Sets configuration options that will be used in all location requests.


```ts
Geolocation.setRNConfiguration(
  config: {
    skipPermissionRequests: boolean;
    locationProvider?: 'playServices' | 'android' | 'auto';
  }
) => void
```

Supported options:

* `skipPermissionRequests` (boolean) - Defaults to `false`. If `true`, you must request permissions before using Geolocation APIs.
* `enableBackgroundLocationUpdates` (boolean, iOS-only) - When using `skipPermissionRequests`, toggle wether to automatically enableBackgroundLocationUpdates. Defaults to true.
* `locationProvider` (string, Android-only) - Either `"playServices"`, `"android"`, or `"auto"`.  Determines wether to use `Google’s Location Services API` or `Android’s Location API`. The `"auto"` mode defaults to `android`, and falls back to Android's Location API if play services aren't available.

---

#### `requestAuthorization()`

Request suitable Location permission. 

```ts
  Geolocation.requestAuthorization(
    success?: () => void,
    error?: (
      error: {
        code: number;
        message: string;
        PERMISSION_DENIED: number;
        POSITION_UNAVAILABLE: number;
        TIMEOUT: number;
      }
    ) => void
  )
```

On iOS if NSLocationAlwaysUsageDescription is set, it will request Always authorization, although if NSLocationWhenInUseUsageDescription is set, it will request InUse authorization.

---

#### `getCurrentPosition()`

Invokes the success callback once with the latest location info.

```ts
  Geolocation.getCurrentPosition(
    success: (
      position: {
        coords: {
          latitude: number;
          longitude: number;
          altitude: number | null;
          accuracy: number;
          altitudeAccuracy: number | null;
          heading: number | null;
          speed: number | null;
        };
        timestamp: number;
      }
    ) => void,
    error?: (
      error: {
        code: number;
        message: string;
        PERMISSION_DENIED: number;
        POSITION_UNAVAILABLE: number;
        TIMEOUT: number;
      }
    ) => void,
    options?: {
        timeout?: number;
        maximumAge?: number;
        enableHighAccuracy?: boolean;
    }
  )
```


Supported options:

* `timeout` (ms) - Is a positive value representing the maximum length of time (in milliseconds) the device is allowed to take in order to return a position. Defaults to 10 minutes.
* `maximumAge` (ms) - Is a positive value indicating the maximum age in milliseconds of a possible cached position that is acceptable to return. If set to 0, it means that the device cannot use a cached position and must attempt to retrieve the real current position. If set to Infinity the device will always return a cached position regardless of its age. Defaults to INFINITY.
* `enableHighAccuracy` (bool) - Is a boolean representing if to use GPS or not. If set to true, a GPS position will be requested. If set to false, a WIFI location will be requested.

---

#### `watchPosition()`

Invokes the success callback whenever the location changes. Returns a `watchId` (number).

```ts
  Geolocation.watchPosition(
    success: (
      position: {
        coords: {
          latitude: number;
          longitude: number;
          altitude: number | null;
          accuracy: number;
          altitudeAccuracy: number | null;
          heading: number | null;
          speed: number | null;
        };
        timestamp: number;
      }
    ) => void,
    error?: (
      error: {
        code: number;
        message: string;
        PERMISSION_DENIED: number;
        POSITION_UNAVAILABLE: number;
        TIMEOUT: number;
      }
    ) => void,
    options?: {
      interval?: number;
      fastestInterval?: number;
      timeout?: number;
      maximumAge?: number;
      enableHighAccuracy?: boolean;
      distanceFilter?: number;
      useSignificantChanges?: boolean;
    }
  ) => number
```

Supported options:

* `interval` (ms) -- (Android only) The rate in milliseconds at which your app prefers to receive location updates. Note that the location updates may be somewhat faster or slower than this rate to optimize for battery usage, or there may be no updates at all (if the device has no connectivity, for example).
* `fastestInterval` (ms) -- (Android only) The fastest rate in milliseconds at which your app can handle location updates. Unless your app benefits from receiving updates more quickly than the rate specified in `interval`, you don't need to set it.
* `timeout` (ms) - Is a positive value representing the maximum length of time (in milliseconds) the device is allowed to take in order to return a position. Defaults to 10 minutes.
* `maximumAge` (ms) - Is a positive value indicating the maximum age in milliseconds of a possible cached position that is acceptable to return. If set to 0, it means that the device cannot use a cached position and must attempt to retrieve the real current position. If set to Infinity the device will always return a cached position regardless of its age. Defaults to INFINITY.
* `enableHighAccuracy` (bool) - Is a boolean representing if to use GPS or not. If set to true, a GPS position will be requested. If set to false, a WIFI location will be requested.
* `distanceFilter` (m) - The minimum distance from the previous location to exceed before returning a new location. Set to 0 to not filter locations. Defaults to 100m.
* `useSignificantChanges` (bool) - Uses the battery-efficient native significant changes APIs to return locations. Locations will only be returned when the device detects a significant distance has been breached. Defaults to FALSE.

---

#### `clearWatch()`

Clears watch observer by id returned by `watchPosition()`

```ts
Geolocation.clearWatch(watchID: number);
```

## Maintainers

This module is developed and maintained by [michalchudziak](https://github.com/michalchudziak).

I owe a lot to the fantastic React & React Native community, and I contribute back with my free time 👨🏼‍💼💻 so if you like the project, please star it ⭐️!

If you need any help with this module, or anything else, feel free to reach out to me! I provide boutique consultancy services for React & React Native. Just visit my [website](https://michalchudziak.dev), or send me an email at [hello@michalchudziak.dev](mailto:hello@michalchudziak.dev) 🙏🏻

### Co-maintainers needed

Due to personal commitments, recently I am unable to dedicate the necessary time to maintain this library as it deserves. I’m looking for passionate contributors to help keep the project alive and thriving. If you're interested in contributing or taking on a maintainer role, please reach out [hello@michalchudziak.dev](mailto:hello@michalchudziak.dev) — your support would mean a lot!

## Contributors

This module was extracted from `react-native` core. Please refer to https://github.com/react-native-community/react-native-geolocation/graphs/contributors for the complete list of contributors.

## License
The library is released under the MIT licence. For more information see `LICENSE`.
