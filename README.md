# react-native-nordic-dfu [![CircleCI](https://circleci.com/gh/Pilloxa/react-native-nordic-dfu.svg?style=svg)](https://circleci.com/gh/Pilloxa/react-native-nordic-dfu) [![Known Vulnerabilities](https://snyk.io/test/github/pilloxa/react-native-nordic-dfu/badge.svg)](https://snyk.io/test/github/pilloxa/react-native-nordic-dfu)

This library allows you to do a Device Firmware Update (DFU) of your nrf51 or 
nrf52 chip from Nordic Semiconductor. It currently only works for Android but 
the iOS functionality is on the way.

WARNING: The API might change when the iOS functionality is added.

For more info about the DFU process, see: [Resources](#resources)

## Getting started

`$ yarn add react-native-nordic-dfu`

### Mostly automatic installation

`$ react-native link react-native-nordic-dfu`

## API

<!-- Generated by documentation.js. Update this documentation by updating the source code. -->

### startDFU

Starts the DFU process

Observe: The peripheral must have been discovered by the native BLE side so that the 
bluetooth stack knows about it. This library will not do a scan but only
the actual connect and then the transfer. See the example project to see how it can be
done in React Native.

**Parameters**

-   `obj` **[Object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object)** 
    -   `obj.deviceAddress` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** The MAC address for the device that should be updated
    -   `obj.deviceName` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** The name of the device in the update notification (optional, default `null`)
    -   `obj.filePath` **[string](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)** The file system path to the zip-file used for updating

**Examples**

```javascript
import { NordicDFU, DFUEmitter } from "react-native-nordic-dfu";

NordicDFU.startDFU({
  deviceAddress: "C3:53:C0:39:2F:99",
  name: "Pilloxa Pillbox",
  filePath: "/data/user/0/com.nordicdfuexample/files/RNFetchBlobTmp4of.zip"
})
  .then(res => console.log("Transfer done:", res))
  .catch(console.log);
```

Returns **[Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)** A promise that resolves or rejects with the `deviceAddress` in the return value

### DFUEmitter

Event emitter for DFU state and progress events

**Examples**

```javascript
import { NordicDFU, DFUEmitter } from "react-native-nordic-dfu";

DFUEmitter.addlistener("DFUProgress",({percent, currentPart, partsTotal, avgSpeed, speed}) => {
  console.log("DFU progress: " + percent +"%");
});

DFUEmitter.addListener("DFUStateChanged", ({state}) => {
  console.log("DFU State:", state);
})
```

## Full Example

See: [example/index.js](example/index.android.js)

## Manual installation

### iOS

1.  In XCode, in the project navigator, right click `Libraries` ➜ `Add Files to [your project's name]`
2.  Go to `node_modules` ➜ `react-native-nordic-dfu` and add `RNNordicDfu.xcodeproj`
3.  In XCode, in the project navigator, select your project. Add `libRNNordicDfu.a` to your project's `Build Phases` ➜ `Link Binary With Libraries`
4.  Run your project (`Cmd+R`)&lt;

### Android

1.  Open up `android/app/src/main/java/[...]/MainActivity.java`

-   Add `import com.pilloxa.RNNordicDfuPackage;` to the imports at the top of the file
-   Add `new RNNordicDfuPackage()` to the list returned by the `getPackages()` method

2.  Append the following lines to `android/settings.gradle`:
        include ':react-native-nordic-dfu'
        project(':react-native-nordic-dfu').projectDir = new File(rootProject.projectDir, 	'../node_modules/react-native-nordic-dfu/android')
3.  Insert the following lines inside the dependencies block in `android/app/build.gradle`:
          compile project(':react-native-nordic-dfu')

## Resources

-   [DFU Introduction](http://infocenter.nordicsemi.com/topic/com.nordic.infocenter.sdk5.v11.0.0/examples_ble_dfu.html?cp=6_0_0_4_3_1 "BLE Bootloader/DFU")
-   [Secure DFU Introduction](http://infocenter.nordicsemi.com/topic/com.nordic.infocenter.sdk5.v12.0.0/ble_sdk_app_dfu_bootloader.html?cp=4_0_0_4_3_1 "BLE Secure DFU Bootloader")
-   [How to create init packet](https://github.com/NordicSemiconductor/Android-nRF-Connect/tree/master/init%20packet%20handling "Init packet handling")
-   [nRF51 Development Kit (DK)](http://www.nordicsemi.com/eng/Products/nRF51-DK "nRF51 DK") (compatible with Arduino Uno Revision 3)
-   [nRF52 Development Kit (DK)](http://www.nordicsemi.com/eng/Products/Bluetooth-Smart-Bluetooth-low-energy/nRF52-DK "nRF52 DK") (compatible with Arduino Uno Revision 3)
