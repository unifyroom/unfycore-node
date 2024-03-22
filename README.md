Unifyroomcore Node
============

A Unifyroom full node for building applications and services with Node.js. A node is extensible and can be configured to run additional services. At the minimum a node has an interface to [Unifyroom Core (unfyd) v0.13.0](https://github.com/unifyroom/unifycoin/tree/v0.13.0.x) for more advanced address queries. Additional services can be enabled to make a node more useful such as exposing new APIs, running a block explorer and wallet service.

## Usages

### As a standalone server

```bash
git clone https://github.com/unifyroom/unfycore-node
cd unfycore-node
npm install
./bin/Unifyroomcore-node start
```

When running the start command, it will seek for `.unfycore/unfycore-node.json` conf file in the working directory (see [/docs/services/unfyd.md](/docs/services/unfyd.md) for an example).
If it doesn't exist, it will create it, with basic task to connect to unfyd.

Some plugins are available :

- Insight-API : `./bin/unfycore-node addservice @unifyroom/insight-api`
- Insight-UI : `./bin/unfycore-node addservice @unifyroom/insight-ui`

You also might want to add these index to your unfy.conf file :
```
-addressindex
-timestampindex
-spentindex
```

### As a library

```bash
npm install @unifyroom/unfycore-node
```

```javascript
const unfycore = require('@unifyroom/unfycore-node');
const config = require('./unfycore-node.json');

let node = unfycore.scaffold.start({ path: "", config: config });
node.on('ready', function () {
    console.log("Unifyroom core started");
    
    node.services.unfyd.on('tx', function(txData) {
        let tx = new unfycore.lib.Transaction(txData);
        console.log(tx);
    });
});
```

## Prerequisites

- Unifyroom Core (unfyd) (v0.13.0) with support for additional indexing *(see above)*
- Node.js v8+
- ZeroMQ *(libzmq3-dev for Ubuntu/Debian or zeromq on OSX)*
- ~50GB of disk storage
- ~1GB of RAM

## Configuration

Unifyroomcore includes a Command Line Interface (CLI) for managing, configuring and interfacing with your Unifyroomcore Node.

```bash
unfycore-node create -d <dash-data-dir> mynode
cd mynode
unfycore-node install <service>
unfycore-node install https://github.com/yourname/helloworld
unfycore-node start
```

This will create a directory with configuration files for your node and install the necessary dependencies.

Please note that [Unifyroom Core](https://github.com/unifyroom/unifycoin/tree/master) needs to be installed first.

For more information about (and developing) services, please see the [Service Documentation](docs/services.md).

## Add-on Services

There are several add-on services available to extend the functionality of Bitcore:

- [Insight API](https://github.com/unifyroom/insight-api/tree/master)
- [Insight UI](https://github.com/unifyroom/insight-ui/tree/master)
- [Bitcore Wallet Service](https://github.com/unifyroom/unfycore-wallet-service/tree/master)

## Documentation

- [Upgrade Notes](docs/upgrade.md)
- [Services](docs/services.md)
  - [Unifyroomd](docs/services/unfyd.md) - Interface to Unifyroom Core
  - [Web](docs/services/web.md) - Creates an express application over which services can expose their web/API content
- [Development Environment](docs/development.md) - Guide for setting up a development environment
- [Node](docs/node.md) - Details on the node constructor
- [Bus](docs/bus.md) - Overview of the event bus constructor
- [Release Process](docs/release.md) - Information about verifying a release and the release process.


## Setting up dev environment (with Insight)

Prerequisite : Having a unfyd node already runing `unfyd --daemon`.

Unifyroomcore-node : `git clone https://github.com/unifyroom/unfycore-node -b develop`
Insight-api (optional) : `git clone https://github.com/unifyroom/insight-api -b develop`
Insight-UI (optional) : `git clone https://github.com/unifyroom/insight-ui -b develop`

Install them :
```
cd unfycore-node && npm install \
 && cd ../insight-ui && npm install \
 && cd ../insight-api && npm install && cd ..
```

Symbolic linking in parent folder :
```
npm link ../insight-api
npm link ../insight-ui
```

Start with `./bin/unfycore-node start` to first generate a ~/.unfycore/unfycore-node.json file.
Append this file with `"@unifyroom/insight-ui"` and `"@unifyroom/insight-api"` in the services array.

## Contributing

Please send pull requests for bug fixes, code optimization, and ideas for improvement. For more information on how to contribute, please refer to our [CONTRIBUTING](https://github.com/unifyroom/unifycoin/blob/master/CONTRIBUTING.md) file.

## License

Code released under [the MIT license](https://github.com/unifyroom/unfycore-node/blob/master/LICENSE).

Copyright 2016-2018 Unifyroom Core Group, Inc.

- bitcoin: Copyright (c) 2009-2015 Bitcoin Core Developers (MIT License)
