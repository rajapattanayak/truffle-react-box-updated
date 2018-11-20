# Truffle React Box Updated

*DAPP is running on http://truffle-react-basic.s3-website-us-east-1.amazonaws.com/
and demo smart contract is deployed to Rinkeby.*

## Setup
 
 * Node should be installed

```
node -v
v10.13.0

```

* truffle should be installed

**npm install -g truffle**

```
truffle -v
Truffle v4.1.14 - a development framework for Ethereum

Usage: truffle <command> [options]

Commands:
  init      Initialize new and empty Ethereum project
  compile   Compile contract source files
  migrate   Run migrations to deploy contracts
  deploy    (alias for migrate)
  build     Execute build pipeline (if configuration present)
  test      Run JavaScript and Solidity tests
  debug     Interactively debug any transaction on the blockchain (experimental)
  opcode    Print the compiled opcodes for a given contract
  console   Run a console with contract abstractions and commands available
  develop   Open a console with a local development blockchain
  create    Helper to create new contracts, migrations and tests
  install   Install a package from the Ethereum Package Registry
  publish   Publish a package to the Ethereum Package Registry
  networks  Show addresses for deployed contracts on each network
  watch     Watch filesystem for changes and rebuild the project automatically
  serve     Serve the build directory on localhost and watch for changes
  exec      Execute a JS module within this Truffle environment
  unbox     Download a Truffle Box, a pre-built Truffle project
  version   Show version number and exit

See more at http://truffleframework.com/docs

```

**truffle commands**

```
  Compile:              truffle compile
  Migrate:              truffle migrate
  Test contracts:       truffle test
  Test dapp:            cd client && npm test
  Run dev server:       cd client && npm run start
  Build for production: cd client && npm run build
```


## Deploy Contracts to Local Blockchain

### Steps:

* Start Ganache Server

* Add/Modify network configuration : truffle.js as per your local and Test setting
    https://truffleframework.com/docs/truffle/reference/configuration#networks

* Change as per ganache running port inside client -> utils -> getWeb3.js (For me line 23 : Port 8545)

* Compile:

    **truffle compile**

```
truffle compile
Compiling ./contracts/Migrations.sol...
Compiling ./contracts/SimpleStorage.sol...
Writing artifacts to ./build/contracts
```

* Migrate Contract to Ganache

    **truffle migrate**

```
truffle migrate
Using network 'development'.

Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0xecf371918cf5827aed837adbc934136c02d6f8db3eed3e61567aa609afa8459c
  Migrations: 0x3e3ae2ad49e0683aff98a296e8c347906ebaa715
Saving successful migration to network...
  ... 0x14f7eed936cf55ec99396c0d84ea0a917a9168da781bee33fd1fef4d40d6ecfc
Saving artifacts...
Running migration: 2_deploy_contracts.js
  Deploying SimpleStorage...
  ... 0xb3327e4c115a7f6f53cf0ffa44d5b238bba0546c51d78047712aa08113020fa1
  SimpleStorage: 0xd6a18656a479d2d369c55779b7c1857404cf299f
Saving successful migration to network...
  ... 0x02831d24e49f58e0cd17b82b5ad30374a65c8d536d82808f89c69e91bb4cc445
Saving artifacts...
```

* Test deployed contracts

    **truffle test**

```
Using network 'development'.

Compiling ./contracts/SimpleStorage.sol...
Compiling ./test/TestSimpleStorage.sol...
Compiling truffle/Assert.sol...
Compiling truffle/DeployedAddresses.sol...


  TestSimpleStorage
    ✓ testItStoresAValue (102ms)

  Contract: SimpleStorage
    ✓ ...should store the value 89. (100ms)


  2 passing (1s)
```


## Run the Frontend Client

Inside Client (cd client)

* Run Test

    **npm test**

```
 PASS  src/App.test.js
  ✓ renders without crashing (17ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        3.475s
Ran all test suites related to changed files.

Watch Usage
 › Press p to filter by a filename regex pattern.
 › Press t to filter by a test name regex pattern.
 › Press q to quit watch mode.
 › Press Enter to trigger a test run.
```

* Run frontend dev server

    **npm run start**

```
 PASS  src/App.test.js
  ✓ renders without crashing (17ms)

Test Suites: 1 passed, 1 total
Tests:       1 passed, 1 total
Snapshots:   0 total
Time:        3.475s
Ran all test suites related to changed files.

Watch Usage
 › Press p to filter by a filename regex pattern.
 › Press t to filter by a test name regex pattern.
 › Press q to quit watch mode.
 › Press Enter to trigger a test run.
```

![Alt text](/images/local-ganche-test.png?raw=true "LocalGancheTest")


## Push Frontend to Production (AWS s3)

**npm run build**

```
Set target browsers: >0.2%, not dead, not ie <= 11, not op_mini all

Creating an optimized production build...
Compiled successfully.

File sizes after gzip:

  292.79 KB             build/static/js/1.29dbdc8d.chunk.js
  3.53 KB (-526.63 KB)  build/static/js/main.d9739162.chunk.js
  763 B                 build/static/js/runtime~main.229c360f.js
  136 B (+5 B)          build/static/css/main.8542fc86.chunk.css

The project was built assuming it is hosted at the server root.
You can control this with the homepage field in your package.json.
For example, add this to build it for GitHub Pages:

  "homepage" : "http://myname.github.io/myapp",

The build folder is ready to be deployed.
You may serve it with a static server:

  yarn global add serve
  serve -s build

Find out more about deployment here:

  http://bit.ly/CRA-deploy
```

- Copy the client/build folder and put it in s3
- Create new s3 bucket
- Give public access. Go to Public access settings . ACL and Bucket policy should be False.
- Upload build files from local box
- Add static website end point.

## Push Contract to Rinkeby

* Update the Production network

Add the config:
* Create .env file in source directory.
    * Enter env details. Get INFURA API key from https://infura.io/
    and MNEMONIC from Metamask.
    ```
			INFURA_APIKEY=""
			MNEMONIC=""
    ```


* Push contract to Rinkeby

    **truffle migrate --reset --compile-all --network rinkeby**

```
truffle migrate --reset --compile-all --network rinkeby
Compiling ./contracts/Migrations.sol...
Compiling ./contracts/SimpleStorage.sol...
Writing artifacts to ./build/contracts

Using network 'rinkeby'.

Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0xd5ca7578a21d6f9d2096b03d385701e3ee5a20f7cbe90b602094de1ebb7ff35d
  Migrations: 0x631ac351fbc929b964f388d70e761843b662fb89
Saving successful migration to network...
  ... 0x7f79ac98b4d97bdec624677479e271b66bb8d01cf9c098eecefbbf04a503f67e
Saving artifacts...
Running migration: 2_deploy_contracts.js
  Deploying SimpleStorage...
  ... 0xd37f1223af0d038105558867f479eb66181b0fd821983e44d41286970385a4d2
  SimpleStorage: 0x39237093f9bf440d9806fb5e7e30fbec062aab3c
Saving successful migration to network...
  ... 0x3f642ed2e6e275fb7a61ed0a497a6ef4b502dfe947debc8cbb9b56317c11c9e7
Saving artifacts...


Test from local frontend:

client: npm run start

Compiled successfully!
You can now view client in the browser.
  Local:            http://localhost:3000/
  On Your Network:  http://10.1.3.37:3000/
```

![Alt text](/images/local-rinkeby-test.png?raw=true "LocalRinkebyTest")

### Test it from Production s3 URL:

* Build frontend for Production. (It will include rinkeby contract address to connect)
* npm run build
* Copy client/build to s3 bucket.
* Go to your s3 bucket serving url

**DAPP in s3 and contracts are in Rinkeby. Both are able to communicate**

![Alt text](/images/s3-rinkeby-test.png?raw=true "s3RinkebyTest")