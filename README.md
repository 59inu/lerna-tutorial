
<table width='100%'>
    <tr>
        <td align='left' width='100%' colspan='2'>
            <strong>Lerna Tutorial</strong><br />
            Learn how to create and mantain monorepos with lerna.
        </td>
    </tr>
    <tr>
        <td>
            A FOSS (Free & Open Source Software) project. Maintained by <a href='https://github.com/ahmadawais'>@AhmadAwais</a>.
        </td>
        <td align='center'>
            <a href='https://AhmadAwais.com/'>
                <img src='https://i.imgur.com/Asg4d3k.png' width='100' />
            </a>
        </td>
    </tr>
</table>

# Lerna tutorial

First off, What is [`lerna`](https://github.com/lerna/lerna)? [`lerna`](https://github.com/lerna/lerna) is a tool that allows you to maintain multiple `npm` packages within one repository.

There's a couple of benefits to this kind of approach, the paradigm is called a `monorepo`, and more can be read about it from the [source of `babel`, and `react`](https://github.com/babel/babel/blob/master/doc/design/monorepo.md).

Here's the gist:

* Single lint, build, test and release process.
* Easy to coordinate changes across modules.
* Single place to report issues.
* Easier to setup a development environment.
* Tests across modules are ran together which finds bugs that touch multiple modules easier.

## Getting started.

For this demo I'm going to install [`lerna`](https://github.com/lerna/lerna) is a CLI (command line interface) tool. You're going to want to install it with the `--global` (`-g`) flag.

```
npm i lerna -g
```

Then once it's done installing your going to want to run the following

```
lerna init
```

This will do a couple of things.

* Creating packages folder.
* Updating package.json.
* Creating lerna.json.

The `/packages` folder is where all of your packages belong. Let's go about making a new package `aa-alpha`.

```
cd packages
mkdir aa-alpha
cd aa-alpha
npm init -y
echo "module.exports = 'aa-alpha'" > index.js
```

Lets go through the same steps for another package `aa-beta`.

First go up one directory:

```
cd ..
```

Now go about creating `aa-beta`.

```
mkdir aa-beta
cd aa-beta
npm init -y
echo "module.exports = 'aa-beta'" > index.js
```

Now we're going to create a `usage` package that uses both `aa-alpha` and `aa-beta` as dependencies.

First go up one directory:

```
cd ..
```

Now go about creating `\usage`.

```
mkdir usage
cd usage
npm init -y
touch index.js
```

Open up `/packages/usage/index.js` in a text editor and paste this in.

```js
var aa-alpha = require('aa-alpha')
var aa-beta = require('aa-beta')
console.log(aa-alpha + " " + aa-beta)
```

We're almost there. At this point your whole project should look something like this:

```
.
├── README.md
├── lerna.json
├── package.json
└── packages
    ├── aa-alpha
    │   ├── index.js
    │   └── package.json
    ├── aa-beta
    │   ├── index.js
    │   └── package.json
    └── usage
        ├── index.js
        └── package.json
```

What you want to do now is go into `/packages/usage/package.json` and add these lines under `dependencies`.

```json
{
  "dependencies": {
    "aa-alpha": "1.0.0",
    "aa-beta": "1.0.0"
  }
}
```

Now you need to wire everything up with this command.

```
lerna bootstrap
```

The output from this command should look something like this:

```
Lerna v2.0.0-aa-beta.20
Linking all dependencies
Successfully bootstrapped 3 packages.
```

Now using the `tree` command once more (`brew install tree`) we can see the folder structure we can see what [`lerna`](https://github.com/lerna/lerna) did.

```
.
├── README.md
├── lerna.json
├── package.json
└── packages
    ├── aa-alpha
    │   ├── index.js
    │   ├── node_modules
    │   └── package.json
    ├── aa-beta
    │   ├── index.js
    │   ├── node_modules
    │   └── package.json
    └── usage
        ├── index.js
        ├── node_modules
        │   ├── aa-alpha
        │   │   ├── index.js
        │   │   └── package.json
        │   └── aa-beta
        │       ├── index.js
        │       └── package.json
        └── package.json
```

It added two `stubbed` (my term not [`lerna`](https://github.com/lerna/lerna)'s) modules. If you peak inside `/packages/usage/node_modules/aa-alpha/index.js` you can see what I mean.

> contents of `./packages/usage/node_modules/aa-alpha/index.js`

```js
module.exports = require("/Users/user/Desktop/lerna-tutorial/packages/aa-alpha");
```

> Note: This is an absolute path to the module. So if you ever move your lerna project you'll need to rerun `lerna bootstrap`.

And volia! When we run `node ./packages/usage/index.js` we get our expected output!

```
aa-alpha aa-beta
```
---

### 🙌 [THEDEVCOUPLE PARTNERS](https://TheDevCouple.com/partners)

This open source project is maintained by the help of awesome businesses listed below. What? [Read more about it →](https://TheDevCouple.com/partners)

<table width='100%'>
	<tr>
		<td width='500'><a target='_blank' href='https://kinsta.com/?kaid=WMDAKYHJLNJX&utm_source=TheDevCouple&utm_medium=Partner'><img src='https://on.ahmda.ws/73cedc/c' /></a></td>
		<td width='500'><a target='_blank' href='https://ahmda.ws/USES_WPE?utm_source=TheDevCouple&utm_medium=Partner'><img src='https://on.ahmda.ws/ff40fe/c' /></a></td>
	</tr>
	<tr>
		<td width='500'><a target='_blank' href='https://mythemeshop.com/?utm_source=TheDevCouple&utm_medium=Partner'><img src='https://on.ahmda.ws/3166d9/c' /></a></td>
		<td width='500'><a target='_blank' href='https://ipapi.co/?utm_source=TheDevCouple&utm_medium=Partner'><img src='https://d2ddoduugvun08.cloudfront.net/items/1R190r2U0p3N3L0U0b2u/ip-api.png'/></a></td>
	</tr>
</table>

<br />
<br />
<p align="center">
<strong>For anything else, tweet at <a href="https://twitter.com/MrAhmadAwais/" target="_blank" rel="noopener noreferrer">@MrAhmadAwais</a></strong>
</p>

<div align="center">
	<p>I have released a video course to help you become a better developer — <a href="https://VSCode.pro/?utm_source=GitHubFOSS" target="_blank">Become a VSCode Power User →</a></p>
    <br />
  <a href="https://VSCode.pro/?utm_source=GitHubFOSS" target="_blank">
  <img src="https://raw.githubusercontent.com/ahmadawais/shades-of-purple-vscode/master/images/vscodeproPlay.jpg" /><br>VSCode</a>

  _<small><a href="https://VSCode.pro/?utm_source=GitHubFOSS" target="_blank">VSCode Power User Course →</a></small>_
</div>
