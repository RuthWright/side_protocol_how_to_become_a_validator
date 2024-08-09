# side_protocol_how_to_become_a_validator

![64d298b0c4a8ecccb47afc86_black](https://github.com/user-attachments/assets/03bbf70a-b229-4830-b839-93cfcf71acd1)

# 🌐 How to Set Up a Side Protocol Node and Become a Validator

Side Protocol is a cutting-edge project focused on enabling secure, scalable, and decentralized applications. By running a Side Protocol Node and becoming a validator, you help maintain network integrity and earn rewards for your contribution. This guide will walk you through the process.

## 🛠 Prerequisites

Before diving into the setup, ensure you have the following:

- **Hardware Requirements**:
  - **CPU**: 4 cores (8 threads recommended)
  - **RAM**: 16 GB or more
  - **Storage**: SSD with at least 500 GB free space
  - **Network**: A stable internet connection with at least 100 Mbps upload/download speed
- **Operating System**: Ubuntu 20.04+ (recommended), macOS, or Windows with WSL2
- **Terminal Knowledge**: Basic familiarity with command-line operations

## 🚀 Step 1: System Preparation

### 1.1 Update Your System

First, make sure your system is fully updated to avoid compatibility issues:

```bash
sudo apt-get update && sudo apt-get upgrade -y
```

### 1.2 Install Dependencies

Next, install the essential tools needed to build and run the Side Protocol Node:

```bash
sudo apt-get install build-essential git curl jq -y
```

### 1.3 Install Go

Side Protocol is built in Go, so you'll need to install it:

```bash
wget https://golang.org/dl/go1.20.7.linux-amd64.tar.gz
sudo tar -C /usr/local -xzf go1.20.7.linux-amd64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc
source ~/.bashrc
```

Verify the installation:

```bash
go version
```

## 📥 Step 2: Download and Build the Side Protocol Node

### 2.1 Clone the Repository

Clone the official Side Protocol repository:

```bash
git clone https://github.com/sideprotocol/side.git
cd side
```

### 2.2 Build the Node

Build the node using the `make` command:

```bash
make build
```

### 2.3 Verify the Installation

After building, confirm the installation by checking the version:

```bash
./side version
```

## ⚙️ Step 3: Node Configuration

### 3.1 Initialize the Node

Initialize the Side Protocol node with a custom moniker (a unique name for your node):

```bash
./side init <your-moniker> --chain-id=<side-chain-id>
```

### 3.2 Download the Genesis File

Download the genesis file that matches the Side Protocol network:

```bash
curl -s <genesis-file-url> > ~/.side/config/genesis.json
```

Replace `<genesis-file-url>` with the actual URL of the genesis file.

### 3.3 Configure Peers and Seeds

Open the `config.toml` file to configure network peers and seeds:

```bash
nano ~/.side/config/config.toml
```

Add peers under the `persistent_peers` section. You can find peers by joining the Side Protocol community or checking documentation.

### 3.4 Set Gas Prices

Set a minimum gas price in `app.toml`:

```bash
nano ~/.side/config/app.toml
```

Locate `minimum-gas-prices` and set it to a value like `0.025uside`.

### 3.5 Optimize Node Performance

You can adjust the pruning settings to save disk space:

```bash
nano ~/.side/config/app.toml
```

Set pruning as follows:

```bash
pruning = "custom"
pruning-keep-recent = "100"
pruning-keep-every = "0"
pruning-interval = "10"
```

## ▶️ Step 4: Start the Node

### 4.1 Launch the Node

Start your node with the following command:

```bash
./side start
```

### 4.2 Check Synchronization

Monitor the synchronization status:

```bash
./side status 2>&1 | jq .SyncInfo
```

It may take some time to sync depending on the blockchain size and your network speed.

## 💼 Step 5: Become a Validator

Once your node is fully synced, you can proceed to become a validator.

### 5.1 Create or Import a Wallet

Create a new wallet or import an existing one:

```bash
./side keys add <wallet-name>
```

Or import an existing key:

```bash
./side keys import <wallet-name> <key-file>
```

### 5.2 Fund Your Wallet

Ensure your wallet has sufficient funds for staking and transaction fees. You can transfer tokens from an exchange or another wallet.

### 5.3 Generate Validator Keys

Generate your validator keys with:

```bash
./side tendermint show-validator
```

### 5.4 Submit the Validator Transaction

Create a new validator by submitting a transaction:

```bash
./side tx staking create-validator \
  --amount=<amount_of_stake> \
  --pubkey=$(./side tendermint show-validator) \
  --moniker=<your-moniker> \
  --chain-id=<side-chain-id> \
  --commission-rate="0.10" \
  --commission-max-rate="0.20" \
  --commission-max-change-rate="0.01" \
  --min-self-delegation="1" \
  --from=<wallet-name> \
  --fees=5000uside
```

Replace placeholders with your details.

### 5.5 Check Validator Status

After submitting, verify your validator’s status:

```bash
./side query staking validator $(./side keys show <wallet-name> --bech val -a)
```

### 5.6 Promote Your Validator

To attract delegators, promote your validator in the Side Protocol community. The more tokens delegated to you, the more rewards you can earn.

## 🔄 Step 6: Node Maintenance and Updates

### 6.1 Regular Monitoring

Regularly check your node’s logs and performance:

```bash
tail -f ~/.side/log/side.log
```

### 6.2 Update Node Software

Keep your node software updated:

```bash
git pull
make build
./side unsafe-reset-all
./side start
```

### 6.3 Backup Your Keys

Regularly backup your wallet and validator keys to ensure they are safe.

## 🎉 Conclusion

By setting up and running a Side Protocol Node, and becoming a validator, you are contributing to the security and decentralization of the Side Protocol network. Stay active in the community and keep your node running smoothly to maximize your contributions and rewards. Welcome to the Side Protocol validator community!
