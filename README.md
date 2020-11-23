# Setup a ETH2 Validator

clone the repo and change into

```
git clone ???
```

Change into the repo and create a environment variable `PROJECT_PATH`

```
cd ???
PROJECT_PATH=${pwd}
```

## Create all folders

Change the directory to the deployment-folder

```
cd deployment-pyrmont
```

Let's create a environment variable `DEPLOYMENT_PATH`

```
DEPLOYMENT_PATH=${pwd}
```

First create all folders

```shell
mkdir -p ${DEPLOYMENT_PATH}/goethereum
mkdir -p ${DEPLOYMENT_PATH}/grafana
mkdir -p ${DEPLOYMENT_PATH}/prometheus
mkdir -p ${DEPLOYMENT_PATH}/prysm/beaconchain
mkdir -p ${DEPLOYMENT_PATH}/prysm/validator/passwords
mkdir -p ${DEPLOYMENT_PATH}/prysm/validator/wallets
mkdir -p ${DEPLOYMENT_PATH}/keys
```

Let's choose a password in order to secure your wallet which will be created later on

```
echo <YOUR_WALLET_PASSWORD> >> ${DEPLOYMENT_PATH}/prysm/validator/password/wallet-password
```

## Get Goerli

## Generate Keys

First install the official `eth2.0-deposit-cli` in order to generate your keys
```
cd ${PROJECT_PATH}
curl -LO https://github.com/ethereum/eth2.0-deposit-cli/releases/download/v1.1.0/eth2deposit-cli-ed5a6d3-linux-amd64.tar.gz
tar -xzf eth2deposit-cli-ed5a6d3-linux-amd64.tar.gz 
rm -f eth2deposit-cli-ed5a6d3-linux-amd64.tar.gz 
cd eth2deposit-cli-ed5a6d3-linux-amd64/
```

Generate new validator keys. Make sure you use the correct chain. For testnet `pyrmont`, for mainnet `mainnet`
```
./deposit new-mnemonic --num_validators 1 --chain pyrmont
```

Copy your keys to the deployment path
```
cp -r <YOUR_KEYS_PATH/validator_keys> ${DEPLOYMENT_PATH}/keys/
```

## Register your wallet

visit the launchpad page and follow the instructions to register a validator. After that import the account with

```
docker-compose -f docker-compose.create-account.yml run validator-import-launchpad
```

You can check your imported account by running
```
docker-compose -f docker-compose.create-account.yml run validator-list-accounts
```

## Start everything up

Start your eth1-client, beacon-chain-client, validator, prometheus and grafana with

```
docker-compose up -d
```
