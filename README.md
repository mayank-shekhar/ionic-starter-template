# Starter App

## Get started
### Node and dependencies
- clone the repo.

- node & npm - http://nodejs.org/download/
  - gulp: `npm install --global gulp` - http://gulpjs.com/
  - bower: `npm install --global bower` - http://bower.io/

- Run `npm install` and `bower install`.

## Running in the browser
```sh
gulp watch
```
Prepares everything for development and opens your default browser. Livereloads your application when changing/adding/deleting files to immediately reflect the changes you make. For your convenience any occurring **ESLint or jsonlint errors** will be presented to you on every livereload.

```sh
gulp watch --no-open
```
If you don't want this task to open your browser, just add the `--no-open` option and navigate to `http://localhost:9000` yourself.

```sh
gulp build
gulp build --force-build # build despite linting errors
gulp build --minify # minifies javascript, CSS, HTML and images.

# this works as well with all cordova build-related commands (as they implicitly run gulp build)
gulp --cordova 'run ios --device' --force-build --minify
```
Usually you don't run this command directly, but it will be implicitly run by any build-related Cordova task `gulp watch-build`. It builds your angular app and moves it to the `www/` folder and performs **ESLint or jsonlint** checks.


# Use Environments
> The environment feature allows you to change sets of API Endpoints, Keys, Tokens, App Behavior Switches, Debugging Options and much more with a single parameter for you gulp tasks.

## Choosing an environment
When you run `gulp watch` or any other task that runs `gulp build` without specifying an environment it will default to the dev environment:
```shell
gulp watch                # defaults to --env=dev
gulp build                # so does this
gulp --cordova 'run ios'  # and any other command that uses gulp build
```
In order to choose an environment explicitly add the `--env` flag, like this:
```shell
gulp watch --env=prod
gulp build --env=prod
gulp --cordova 'run ios' --env=prod
```
While you're running `gulp watch` you can even **temporarily** switch the environment you're currently working on without having to restart your watch task. Simply type:
```shell
gulp environment --env=<env>
```
Gulp will livereload with your new environment! It's **important** to note that as soon as you are making more changes and a livereload is triggered, your environment switches back to the one that was supplied when `gulp watch` was started. If you want to **permanently** switch your environment you should do so by restarting your `gulp watch` tasks with the desired environment.

## Creating a new environment
If you find yourself faced needing more than a dev and a prod environment simply create a new file: e.g. `app/main/constants/env-env5.json`, fill it with the desired values and then run one the following:
```shell
gulp watch --env=env5
gulp build --env=env5
gulp environment --env=env5
```

#### Cordova CLI wrapper
```sh
gulp --cordova '<run any cordova command>'
```
A wrapper for **local installation** of Cordova CLI that comes with the generator. As opposed to a global installation the local installation allows you to have different projects with different CLI versions, which happens a lot if you have several projects with different schedules. Additionally this gulp wrapper can trigger additional gulp commands, significantly reducing the number of commands it takes to build your app. You'll learn about this in a minute.

How to use it?

```sh
# regular cordova command (when installed globally).
# DON'T USE!
cordova plugin add org.apache.cordova.camera
# cordova command with local wrapper
gulp --cordova 'plugin add org.apache.cordova.camera --save'
```

Find more plugins on the [Cordova Website](https://cordova.apache.org/plugins/) or on [ngCordova](http://ngcordova.com/docs/plugins/) and find a full list of supported platforms [here](https://cordova.apache.org/docs/en/latest/guide/platforms/index.html).

#### Cordova build, run, emulate, ... under the hood

```sh
gulp --cordova '<build related task>' # runs gulp build, then cordova command
```

If you run one of the commands above or following Cordova commands: `build <platform>`, `run <platform>`, `emulate <platform>`, `serve` or `prepare <platform>` then under the hood `gulp build` will build your Ionic app into the `www/` folder, then the Cordova command will take it from there.

So running:
```sh
gulp --cordova 'run android'
# is exactly the same as
gulp build
gulp --cordova 'run android' --no-build
```
So as you can see the implicit run of `gulp build` that comes with any of the above mentioned Cordova commands can be disabled by adding the `--no-build` flag. This can be handy if you don't make any changes to the source files or just want to be more verbose to make it more obvious what's happening.
```sh
gulp --cordova 'run ios --device' --no-build # no gulp build, only cordova run ios
gulp --cordova 'run android' --no-build # same for android or any other platform
```

#### Run on device or simulator
If you want to run your app on your connected iOS or Android device or a simulator run:
```sh
gulp --cordova 'run ios --device' # runs gulp build, then cordova run ios
gulp --cordova 'run android' # same for android

```
or
```sh
gulp --cordova 'emulate ios'
gulp --cordova 'emulate android'
```

Both commands require your system to be setup correctly for the given platform as described in the Cordova [Platform Guides](http://cordova.apache.org/docs/en/latest/guide/platforms/index.html).

To emulate a specific device and iOS version (iOS version needs to be installed via Xcode) run:
```sh
gulp --cordova 'emulate ios --target="iPad-Air, 8.4"'
gulp --cordova 'emulate ios --target="iPad-Air, 9.0"'
# to list available targets on your machine, run:
`./platforms/ios/cordova/lib/list-emulator-images`
```

#### Run using Xcode instead of command line
Some people prefer to launch their apps (device or emulator) using the Xcode project that lies in `platforms/ios/`. This is sometimes handy because [ios-deploy](https://github.com/phonegap/ios-deploy) which is used by the Cordova CLI to run your apps via the command line can have its quirks.

In order to view changes that you made in the project run the following:
```sh
gulp --cordova 'prepare ios'
# or individually
gulp build
gulp --cordova 'prepare ios' --no-build
```
Then run or simulate via Xcode.


