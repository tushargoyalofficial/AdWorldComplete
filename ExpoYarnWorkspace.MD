# This is how i to make expo work with expo-yarn-workspaces.

## In my global package.json file i added the following codes.

```json
{
  "private": true,
  "workspaces": ["packages/*"]
}
```

then i created a folder in my root directory called packages.

## In my terminal i cd packages then i created a new expo project with this command

```sh
$ expo init app
```

## Define the name and version properties in package.json. then i installed all my dependencies:

```sh
npm install
```

or

```sh
yarn
```

## now its time to install expo-yarn-workspaces with

```sh
npm install --save-dev expo-yarn-workspaces
```

or

```sh
yarn add expo-yarn-workspaces -D
```

## After that you add this script in your package.json

```json
"scripts": {
...,
"postinstall": "expo-yarn-workspaces postinstall"
},
```

## create a file and name it metro.config.js and paste the following code

```js
const { createMetroConfiguration } = require("expo-yarn-workspaces");

module.exports = createMetroConfiguration(\_\_dirname);
```

## in your package.json replace/add this line of code

```json
"main": "**generated**/AppEntry.js",
```

## Run:

```sh
npm run postinstall
```

or

```sh
yarn postinstall
```

## then you can start your app with the following command to clear the cache:

```sh
npm start --clear
```

or

```sh
yarn start --clear
```

Useful links:

[Article](https://divinehycenth.com/blog/yarn-workspace-monorepo)

[Github](https://github.com/expo/expo/tree/master/packages/expo-yarn-workspaces)
