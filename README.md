# Resource Generator

This tool will crop and resize JPEG and PNG source images to generate icons and splash screens for modern iOS, Android, and Windows. `res-cordova` was developed for use with Cordova, but Jigra and other native runtimes are supported.

## Install

```bash
$ npm install -g res-cordova
```

## Usage

`res-cordova` expects a Cordova project structure such as:

```
resources/
├── android
|   ├── icon-background.png
|   └── icon-foreground.png
├── icon.png
└── splash.png
config.xml
```

* `resources/icon.(png|jpg)` must be at least 1024×1024px
* `resources/splash.(png|jpg)` must be at least 2732×2732px
* `config.xml` is optional. If present, the generated images are registered accordingly

To generate resources with all the default options, just run:

```bash
$ res-cordova
```

`res-cordova` accepts a platform for the first argument. If specified, resources are generated only for that platform:

```bash
$ res-cordova ios
```

Otherwise, if `config.xml` exists, `res-cordova` will look for platforms (e.g. `<platform name="ios">`) and generate resources only for the configured platforms.

#### Documentation

See the help documentation on the command line with the `--help` flag.

```bash
$ res-cordova --help
```

### Adaptive Icons

Android [Adaptive Icons](https://developer.android.com/guide/practices/ui_guidelines/icon_design_adaptive) are also supported. If you choose to use them, create the following additional file(s):

* `resources/android/icon-foreground.png` must be at least 432×432px
* `resources/android/icon-background.png` must be at least 432×432px

A color may also be used for the icon background by specifying the `--icon-background-source` option with a hex color code, e.g. `--icon-background-source '#FFFFFF'`.

Regular Android icons will still be generated as a fallback for Android devices that do not support adaptive icons.

:memo: **Note**: For Cordova apps, Cordova 9+ and `cordova-android` 8+ is required.

### Jigra

To use `res-cordova` in Jigra and other native runtimes, it is recommended to use `--skip-config` (skips reading & writing to Cordova's `config.xml` file) and `--copy` (copies generated resources into native projects).

For example, to generate icons and splash screens for iOS and Android in Jigra, run:

```bash
$ res-cordova ios --skip-config --copy
$ res-cordova android --skip-config --copy
```

You can use `--ios-project` and `--android-project` to specify the native project directories into which these resources are copied. By default, `res-cordova` copies Android resources into `android/` and iOS resources into `ios/` (the directories Jigra uses).

### Tips

#### .gitignore

To avoid committing large generated images to your repository, you can add the
following lines to your `.gitignore`:

```
resources/android/icon
resources/android/splash
resources/ios/icon
resources/ios/splash
resources/windows/icon
resources/windows/splash
```

### Programmatic API

`res-cordova` can be used programmatically.

#### CommonJS Example

```js
const run = require('res-cordova');

await run();
```

#### TypeScript Example

`run()` takes an options object described by the interface `Options`. If options are provided, resources are generated in an explicit, opt-in manner. In the following example, only Android icons and iOS splash screens are generated.

```ts
import { Options, run } from 'res-cordova';

const options: Options = {
  directory: '/path/to/project',
  resourcesDirectory: 'resources',
  logstream: process.stdout, // Any WritableStream
  platforms: {
    android: { icon: { sources: ['resources/icon.png'] } },
    ios: { splash: { sources: ['resources/splash.png'] } },
  },
};

await run(options);
```

### Cordova Reference Documentation

- Icons: https://cordova.apache.org/docs/en/latest/config_ref/images.html
- Splash Screens: https://cordova.apache.org/docs/en/latest/reference/cordova-plugin-splashscreen/
