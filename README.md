# Steps to Developer install for smartfacecloud

## Setup your workspace
Create a new C9 workspace or create an empty folder to workwith.
All your code will be used accordingly, except global installation.

## Get C9
```
git clone git://github.com/c9/core.git c9sdk
cd c9sdk
scripts/install-sdk.sh
```

## Get Smartface config
Smartface Config file can be found on [github](https://github.com/c9/smf.core/blob/master/configs/client-workspace-smartface.js).
Download this file and put it under c9sdk/configs

On line 5 following code should be updated
```javascript
//old
var config = require("configs/client-default-hosted")(options);
```

```javascript
//new
var config = require("configs/client-default")(options);
```

While sending the updated config file to C9, you or C9 may modify the file to revert that change.

**Note:** client-default-hosted is C9's own file used in production, we do not have access to it.

## Get Smartfaceplugins
Use following script to get smartface plugins.

*If you are not already in c9sdk folder, change directory to c9sdk. The script below provided accordingly.*

```
cd plugins
mkdir -p @smartface
cd @smartface
git clone https://github.com/SmartfaceIO/smartface.publish.wizard.git smartface.publish.wizard
git clone https://github.com/SmartfaceIO/smartface.about.git smartface.about
git clone https://github.com/SmartfaceIO/smartface.ide.theme.git smartface.ide.theme
git clone https://github.com/SmartfaceIO/smartface.welcome.git smartface.welcome
git clone https://github.com/SmartfaceIO/smartface.language.git smartface.language
```

If you want to add more plugins to list, update the script above.

### C9 shortcut
If you are working on C9 workspace environment to develop smartfacecloud IDE,
you can run the following command to add a shortcut smartface plugins to
favorites:
```
c9 ./
```
or
```
c9 c9sdk/plugins/@smartface
```

## Whitelist C9 plugins
Open c9sdk/config/standalone.js
In this file search for *whitelist*.
In side that object definition, add ```"@smartface": true```

It will be something like this:
```javascript
whitelist: {
    "c9.core": true,
    "c9.fs": true,
    "c9.automate": true,
    "c9.login.client": true,
    "c9.vfs.client": true,
    "c9.cli.bridge": true,
    "c9.nodeapi": true,
    "c9.ide.experiment": true,
    "saucelabs.preview": true,
    "salesforce.sync": true,
    "salesforce.language": true,
    "@smartface": true
}
```

## Setup smartface workspace
Change directory to root of your workspace. On the same level of c9sdk you will
create another folder for target workspace
```
git clone https://github.com/SmartfaceIO/smfc-empty-workspace.git workspace
```

## Setup CLI
```
npm i -g smartface
npm i -g smartface-c9
```

**!!Do not do the following unless if you experience running smfc!!**
```
nvm use 0.12
nvm alias default 0.12
```

### C9 Shortcut
If you want to access C9 folder of globaly installed node modules you can run
following script to add a shortcut to favorites

Learn path of the node
```
which node
```
It will provide you a path like this: */home/ubuntu/.nvm/versions/node/v4.2.1/bin/node*

Remove the bin and node from the path and change directory adding lib/node_modules
If you path is like this, you can add following code to add the shortcut.
```
c9 /home/ubuntu/.nvm/versions/node/v4.2.1/lib/node_modules
```


## Run your C9 IDE
You can run the node & server following path. For C9 environment you have to remove node it for the runner:
```
node c9sdk/server.js -p 8080 -l 0.0.0.0 -a : -w ../workspace --workspacetype=smartface
```