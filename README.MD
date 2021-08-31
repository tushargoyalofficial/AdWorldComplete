# AddWorld Complete Codebase Yarn Workspace

### Follow Link:

[KPITENG Share Code b/w React and React Native](https://dev.to/kpiteng/share-code-between-react-native-and-react-js-50bj)

## Yarn Workspaces

Yarn Workspaces allow developers to create workspaces that share packages under the same workspace. Consider you have multiple packages and all are using common dependencies like graphql, axios, etc. To install dependency separately for each package (node_modules), Yarn Workspaces allow you to install packages at root level and you can access that dependency throughout all packages inside that workspaces. So with lots of installation, memory size will be reduced.

## WML

Consider you have created a few common packages that you need to use for both React, React Native, Server (BackEnd), So whenever you change on common packages you need to add that updated package into your React, React Native node_modules. WML helps you to link your packages to where it is being used.

```properties
> wml add common /app/node_modules/@sharecode/common
```

### This command line will copy common packages inside the app's node_modules.

## nohoist

As discussed earlier in Yarn Workspaces, whatever dependency (npm, node_modules) you were installing for your packages, it will be installed on your root node_modules instead of individual packages. If you checked then react-native packages refer to node_modules inside /app directory. But actually it will be installed on root (node_modules) so to link react-native dependency we are using nohoist which helps you to link your dependency. To do this you need to add few lines of code in your /app/package.json

```json
"workspaces": {
  "nohoist": [
    "react-native",
    "react-native/**",
    "react",
    "react/**"
  ]
}
```

## Steps by Step Integration

1. Create WorkSpace directory named - AdWorldComplete

```properties
> mkdir AdWorldComplete
> cd AdWorldComplete
```

2. Create package.json file and add following lines of code, which contains a list of packages under WorkSpace

```json
{
  "private": true,
  "name": "add-world",
  "version": "1.0.0",
  "workspaces": ["app", "web", "common"]
}
```

Here, we have set up three packages (app, web, common) inside our Workspaces. We have app (React Native App), web (React.js Website), common (Common Code - Which used in React Native & React.js Website)

Now, let's create three projects, app (react native), web (react.js), common (common code between react & react native)

```properties
> npx react-native init app // this will create react native application
> npx create-react-app web // this will create react.js application

> mkdir common // this is our common directory for code share
> cd common

> yarn init -y // this will create package.json file with following
```

```properties
cd /common/package.json
```

```json
{
   "name": "common",
   "version": "1.0.0",
   "description": "...",
   …
}
```

## Now, let's change some hierarchy of folder structure. Right now if you see,

```properties
> ls // it will print following
```

```console
app web common package.json
```

You see, everything are on root folder, let's move app, web, common inside packages folder

```properties
> mkdir packages

> mv app/ packages/app/
> mv web/ packages/app/
> mv common/ packages/app/
```

## You can directory drag your app, web, common folder directory to the packages folder. We have changed folder hierarchy, so we need to update root package.json according

Update AdWorldComplete/package.json file look like this

```json
{
  "private": true,
  "name": "add-world",
  "version": "1.0.0",
  "workspaces": ["packages/*"]
}
```

Till, all going well, If you see packages name it will be like “name”: “app”, “name”: “web”, “name”: “common”, but as per best coding practices we need to append project name, workspace name. So, lets change packages name

## react-share-code/packages/app/package.json -

```gitattributes
{
   - "name": "app",
   + "name": "@sharecode/app"
}
```

## react-share-code/packages/web/package.json -

```gitattributes
{
   - "name": "web",
   + "name": "@sharecode/web"
}
```

## react-share-code/packages/common/package.json -

```gitattributes
{
   - "name": "common",
   + "name": "@sharecode/common"
}
```

## So, we have finish with

- WorkSpace Creation
- Project Creation (React Native, React.JS)
- Common Folder Creation
- Structure Folder Hierarchy Into packages directory
- Updated packages name

Now, let's put some reusable code into a common directory.

Create file index.js inside common directory and add following line -

```javascript
export const sharedVariable = “Shared Variable”;
```

Now, let's use this in React Native Project, to use code we need to add it as a dependency, So lets update /packages/app/package.json file

```json
{
   "devDependencies": {
    + "@sharecode/common": "1.0.0"
   },
   + "workspaces": {
      + "nohoist": [
         + "react-native",
         + "react-native/**",
         + "react",
         + "react/**"
      + ]
   + }
}
```

Here, we have added @sharecode/common as a devDependency and added workspaces - nohoist to get reference of react-native.

```javascript
+ import {sharedVariable} from “@sharecode/common”;

return (
   + <Text>{sharedVariable}</Text>
)
```

Before start running project, let’s remove node_modules both /app and root /node_modules to reinstall packages

```properties
app > rm -rf node_modules ../../node_modules
app > yarn install // install node_modules

```

Now, let's run the react-native app, you will see Text Value “Shared Variable” which is fetched from common logic.

Same way, lets integrate it into React.js Application.

```sh
cd /web/index.js
```

```javascript
+ import {sharedVariable} from “@sharecode/common”;

return (
   + <div>{sharedVariable}</div>
)
```

Run the react.js application you will get printed “Shared Variable”

That’s it, you have used the same code between React Native & React.JS.

## What kind of code you can add into shared/common -

- Redux/Redux Saga/Redux Rematch/Apollo Client/Mobx
- Common API Imports
- Common Themes/ Fonts Settings
- Common Variables/ Constants
- Common API Calls

You can add as much code as you require as per project needs.

Now, If you have changed anything in your common folder/ code, then to merge your latest common packages we are using WML.

```sh
app > wml ../common ./node_modules/@reactsharecode/common
```

Above command will copy code files of common into app/web node_modules under @reactsharecode/common packages.

To start linking you can do

```sh
app > wml start
```

Execute above WML commands whenever you have updated anything into your common packages, WML will merge code into app/node_modules/@reactsharecode/common and web/node_modules/@reactsharecode/common.
