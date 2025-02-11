# Getting Started

This document provides instructions for running a single node network on your local machine and then submitting your first few transactions to that network using the command line. Running a single node network is a great way to get familiar with ixo Blockchain and its functionality.

### Prerequisites

In order to install the `ixo` binary, you'll need the following:

* Git `>=2`
* Make `>=4`
* Go `>=1.17`

For more information, see Prerequisites.

_Note: The `ixo` binary is installed into `$(go env GOPATH)/bin`, so make sure `$(go env GOPATH)/bin` is in your PATH (e.g. `export PATH=$(go env GOPATH)/bin:$PATH`)._

### Install IXO

The `ixo` binary serves as the node client and the application client. In other words, the `ixo` binary can be used to both run a node and interact with it.

Clone the `ixo` repository:

```bash
git clone https://github.com/ixofoundation/ixo-blockchain
```

Change to the `genesis` directory:

```bash
cd ixo-blockchain
```

Check out the latest stable version:

```bash
git checkout master
```

Install the `ixo` binary:

```bash
make install
```

Check to make sure the install was successful:

```bash
ixod version
```

You should see `v1.0.8` printed to the console. Now that you have successfully installed the `ixo` binary, the next step will be to add a couple test accounts.

### Quickstart

If you would like to learn about the setup process and manually set up a single node network, skip to the next section. Alternatively, you can run the following quickstart script:

```bash
./scripts/start_testnode.sh
```

The script provides two command-line options for specifying a keyring-backend (`-k`), and the name of the blockchain (`-c`). For example, to use the `os` keyring-backend with the name `demo`:

```bash
./scripts/start_testnode.sh -k os -c demo
```

After running the quickstart script, you can skip to Start Node.

### Create Accounts

In this section, you will create two test accounts. You will name the first account `validator` and the second account `delegator`. You will create both accounts using the `test` backend, meaning both accounts will not be securely stored and should not be used in a production environment. When using the `test` backend, accounts are stored in the home directory (more on this in the next section).

Create `validator` account:

```bash
ixod keys add validator --keyring-backend test
```

Create `delegator` account:

```bash
ixod keys add delegator --keyring-backend test
```

After running each command, information about each account will be printed to the console. The next step will be to initialize the node.

### Initialize Node

Initializing the node will create the `config` and `data` directories within the home directory. The `config` directory is where configuration files for the node are stored and the `data` directory is where the data for the blockchain is stored. The default home directory is `~/.ixod`.

Initialize the node:

```bash
ixod init node --chain-id test
```

In this case, `node` is the name (or "moniker") of the node and `test` is the chain ID. Feel free to change these values but make sure to use the same value for `chain-id` in the following steps.

### Update Genesis

When the node was initialized, a `genesis.json` file was created within the `config` directory. In this section, you will be adding two genesis accounts (accounts with an initial token balance) and a genesis transaction (a transaction that registers the validator account in the validator set).

Update native staking token to `uixo`:

_For Mac OS:_

```bash
sed -i "" "s/stake/uixo/g" ~/.ixod/config/genesis.json
```

_For Linux variants:_

```bash
sed -i "s/stake/uixo/g" ~/.ixod/config/genesis.json
```

Add `validator` account to `genesis.json`:

```bash
ixod add-genesis-account validator 5000000000uixo --keyring-backend test
```

Add `delegator` account to `genesis.json`:

```bash
ixod add-genesis-account delegator 2000000000uixo --keyring-backend test
```

Create genesis transaction:

```bash
ixod gentx validator 1000000uixo --chain-id test --keyring-backend test
```

Add genesis transaction to `genesis.json`:

```bash
ixod collect-gentxs
```

Now that you have updated the `genesis.json` file, you are ready to start the node. Starting a node with a new genesis file will create a new blockchain.

### Start Node

Well, what are you waiting for?

Start the node:

```bash
ixod start
```

You should see logs printed in your terminal with information about services starting up followed by blocks being produced and committed to your local blockchain.

### Test Commands

Now that you have a single node network running, you can open a new terminal window and interact with the node using the same `ixo` binary. Let's delegate some `uixo` tokens to the validator and then collect the rewards.

Get the validator address for the `validator` account:

```bash
ixod keys show validator --bech val --keyring-backend test
```

Using the validator address, delegate some `uixo` tokens:

```bash
ixod tx staking delegate [validator_address] 10000000uixo --from delegator --keyring-backend test --chain-id test
```

In order to query all delegations, you'll need the address for the `delegator` account:

```bash
ixod keys show delegator --keyring-backend test
```

Using the address, query all delegations for the `delegator` account:

```bash
ixod q staking delegations [delegator_address]
```

Query the rewards using the delegator address and the validator address:

```bash
ixod q distribution rewards [delegator_address] [validator_address]
```

Withdraw the rewards:

```bash
ixod tx distribution withdraw-all-rewards --from delegator --keyring-backend test --chain-id test
```

Check the account balance:

```bash
ixod q bank balances [delegator_address]
```

You have successfully delegated `uixo` tokens to the `validator` account from the `delegator` account and then collected the rewards.
