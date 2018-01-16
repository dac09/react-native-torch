# react-native-torch

[![npm version](https://badge.fury.io/js/react-native-torch.svg)](http://badge.fury.io/js/react-native-torch)

A simple React Native plugin to switch a flashlight on/off.

Currently supports both iOS (>= 8.0) and Android (all versions).

Applies the permission checks on Android API >=25 devices. Below API 25, Android will automatically require the user to accept permissions on install / update.

## Install

```shell
npm install --save react-native-torch
react-native link react-native-torch
```

## Usage

### Without permissions check

```js
import Torch from 'react-native-torch';

Torch.switchState(true); // Turn ON
Torch.switchState(false); // Turn OFF
```

### With extra permission check and dialog (Android only)

```js
import Torch from 'react-native-torch';
import { Platform } from 'react-native';

if (Platform.OS === 'ios') {
	Torch.switchState(this.isTorchOn);
} else {
	const cameraAllowed = await Torch.requestCameraPermission(
		'Camera Permissions', // dialog title
		'We require camera permissions to use the torch on the back of your phone.' // dialog body
	);

	if (cameraAllowed) {
		Torch.switchState(this.isTorchOn);
	}
}
```

### Android catch exception accessing Torch e.g. in emulator or if device doesn't have a torch

```js
const cameraAllowed = await Torch.requestCameraPermission(
	'Camera Permissions', // dialog title
	'We require camera permissions to use the torch on the back of your phone.' // dialog body
);

try {
	const output = await Torch.switchState(newTorchState);
	this.setState({ isTorchOn: newTorchState });
} catch (e) {
	ToastAndroid.show(
		'We seem to have an issue accessing your torch',
		ToastAndroid.SHORT
	);
}
```

NOTE:
iOS fails silently, on Android, you can still call without the try/catch block and it won't cause a crash

A demo application [TorchDemo](https://github.com/ludo/TorchDemo) is also
available.

Android version has flow support.
