## Init

```sh
yarn install
```

## The obstacle

```sh
yarn workspace client-native run android # Fails
```

This one fails because of jetifier, which cannot resolve node-modules.

```
info Running jetifier to migrate libraries to AndroidX. You can disable it using "--no-jetifier" flag.
internal/fs/utils.js:298
    throw err;
    ^

Error: ENOENT: no such file or directory, scandir 'node_modules'
    at readdirSync (fs.js:981:3)
```

To postpone this issue, we can use `--no-jetifier` for now and see if anything else is blocking:

```sh
yarn workspace client-native run android --no-jetifier
```

The command launches the simulator, but fails later with this error:

```
FAILURE: Build failed with an exception.

* Where:
Settings file '/home/xxxx/Programmation/yarn-workspaces/packages/client-native/android/settings.gradle' line: 2

* What went wrong:
A problem occurred evaluating settings 'ClientNative'.
> Could not read script '/home/xxxx/Programmation/yarn-workspaces/packages/client-native/node_modules/@react-native-community/cli-platform-android/native_modules.gradle' as it does not exist.
```

So I thought **nohoist** could be the solution, and I configured the client-native package.json to set it up. Unfortunately, the node linker doesn't seem to follow those rules.
