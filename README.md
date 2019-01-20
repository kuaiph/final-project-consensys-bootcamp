# final-project-DavidMinarsch


## Description
This application allows users who have an Ethereum account to prove the existence of a file at a specific point in time by registering an IPFS hash of the file on the Ethereum blockchain.

To preserve the privacy of the user's file it is not permanently stored. The file is only temporarily stored (in the client and the IPFS node) to generate the IPFS hash. The generated IPFS hash and a time stamp are stored in a smart contract that can be referenced at a later date to verify the existence of the file at the referenced time.

Implicit to the above use case of proof of existence are other use cases like file integrity checking.

User stories:
- A user visits the app from a web3 enabled browser or web2 browser with a Metamask extension. The user must have at least one account registered in their wallet. By default the application uses the first account address registered in the wallet. The user can upload a file to the app and initiate a transaction which references the IPFS hash of the file and a time stamp on the blockchain.

- The app reads the user's account address and shows all the previous registrations associated to this address. The registration information includes the IPFS hash and the timestamp.

- The user can check if a given file has already been registered and at which time by uploading it to the app. If the file has already been registered by the user or another user then the app will show the registration information (IPFS hash and timestamp) and a link to the relevant account address which has registered the file.

The app is deployed on the Rinkeby test net and served from IPFS at:


## Style guide
This project's solidity code follows:
https://solidity.readthedocs.io/en/develop/style-guide.html

## Setup:
Go to root folder and ensure Node.js version is aligned:
```
nvm install
```
Install all dependencies for contract development:
```
npm install
```
Install all dependencies for client development:
```
cd client && npm install
```

## For development (non-upgradable route):
0)
Delete all historic compiled contracts:
```
rm -rf client/src/contracts/*
```
1) Start a test blockchain (in deterministic mode: so it generates deterministic addresses based on a pre-defined mnemonic) with ganache:
```
ganache-cli --deterministic
```
2) In a second terminal window test contracts for expected behaviour:
```
truffle test
```
3) Compile contracts:
```
truffle compile
```
4) Migrate contracts onto the test blockchain:
```
truffle migrate
```
5) Test dapp:
```
cd client && npm test
```
6) Start the dev-server
```
cd client && npm run start
```
7) Make sure MetaMask points to correct network and has your account loaded.

## For development (upgradable route utilizing Zeppelin-OS)
0)
Delete all historic compiled contracts:
```
rm -rf client/src/contracts/*
```
1) Start a test blockchain (in deterministic mode: so it generates deterministic addresses based on a pre-defined mnemonic) with ganache:
```
ganache-cli --deterministic
```
Take the second account address listed and add it to a .env file under OWNER_ADDRESS
```
export OWNER_ADDRESS=0xffcf8fdee72ac11b5c542428b35eef5769c409f0
```
2) In a second terminal window test contracts for expected behaviour:
```
npx truffle test
```
3) Compile and add the contracts to zeppelin os project:
```
npx zos add ProofOfExistence
```
4) Migrate contracts onto the test blockchain :
a) First, create a session (note, the from address MUST be different from the first address generated by ganache!; e.g. take the second one):
```
npx zos session --network development --from $OWNER --expires 3600
```
b) Second, create logic contract:
```
npx zos push
```
c) Third, create first usable upgradeable instance (proxy) of ProofOfExistence contract:
```
npx zos create ProofOfExistence --init initialize
```
5) Test dapp:
```
cd client && npm test
```
6) Start the dev-server
```
cd client && npm run start
```
7) Make sure MetaMask points to correct network and has your account loaded.

### Adding contracts to Zeppelin:
Adding the contract to zeppelin os project:
```
zos add ProofOfExistence
```
Upgrading:
https://docs.zeppelinos.org/docs/upgrading.html

### Using truffle console:
You can interact with the deployed contracts directly via Truffle console:
```
const account = web3.eth.getAccounts().then(a => {return a[0];})
const poe = await ProofOfExistence.at(ProofOfExistence.address)
const c
const ids = await poe.getIdsForAddress.call(await web3.eth.getAccounts().then(a => {return a[0];}))
```

### Linting:
You can run a linting script (first make `lint_all` executable: `chmod +x lint_all`) which runs the common linters:
```
./lint_all

```
More linters (some with issues due to React/Zos setup):
```
myth -x contracts/ProofOfExistence.sol
myth --truffle
slither contracts/ProofOfExistence.sol
```

## For production:
Build for production:
```
cd client && npm run build
```

### Deployment to IPFS

1. Change environment variable to `production`

2. Check webpack to make sure all compression settings are correct

3. Run
```
$ ipfs daemon
# and
$ ipfs add -r public
```


Access mobile cam from web app:
https://stackoverflow.com/questions/8581081/how-to-access-a-mobiles-camera-from-a-web-app
https://www.html5rocks.com/en/tutorials/getusermedia/intro/
https://www.smashingmagazine.com/2018/04/audio-video-recording-react-native-expo/
