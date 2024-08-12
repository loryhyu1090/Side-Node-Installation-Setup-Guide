# Node-Installation-Setup-Guide
This guide provides step-by-step instructions for installing and configuring a blockchain node using the sided binary. It covers everything from the hardware requirements and necessary software dependencies to building the binary from source, configuring the node, and running it as a full node in the Side Protocol network.

![side-bg](https://github.com/user-attachments/assets/82810345-a1ea-4910-81e3-4078c0926c9a)

## **Node Installation & Setup** 🛠️

This guide provides instructions for installing and configuring the `sided` binary. Follow the steps below to set up your node successfully.

### **Hardware Specifications** 💻

**Minimum Requirements:**
- **CPU:** 4 cores
- **RAM:** 8 GB
- **Storage:** 500 GB
- **Network:** 1 Gbps

**Recommended Specifications:**
- **CPU:** 8 cores
- **RAM:** 16 GB
- **Storage:** 800 GB
- **Network:** 1 Gbps

### **Operating System** 🖥️

You can compile the `sided` daemon on various modern Linux distributions and recent versions of macOS. This tutorial assumes you are using an Ubuntu LTS release. If you use a different OS, adjust the commands accordingly.

### **Prerequisites** 📋

- **Golang v1.22.0:** Ensure you have the correct version of Golang installed. Visit the [Golang releases page](https://go.dev/dl/) for instructions.

### **Build `sided` from Source** 🏗️

1. **Check Go Version:**
    ```bash
    go version
    ```
    Make sure the output matches the Golang version specified in the Prerequisites section.

2. **Clone the Source Code:**
    ```bash
    # NOTE: Please remove any previous 'side' directory on your local machine and re-clone the repository.

    git clone https://github.com/sideprotocol/side.git
    cd side
    git checkout v0.9.0
    ```

3. **Compile the `sided` Binary:**
    ```bash
    make install
    ```
    This command compiles the `sided` binary and saves it in your `$GOBIN` directory. If `$GOBIN` is in your `$PATH`, you should be able to execute the `sided` binary.

4. **Set Up Your Environment:**
    ```bash
    export GOPATH=$HOME/go
    export PATH=$PATH:$GOPATH/bin
    ```

5. **Verify Installation:**
    ```bash
    sided version
    ```
    The version should be `0.9.0`. If you encounter any issues with PATH settings, refer to the [Golang releases page](https://go.dev/dl/).

### **Run a Full Node** 🚀

#### **Setting up the Keyring (Optional)** 🔐

The keyring holds the private/public keypairs used to interact with a node. Set up a validator key to sign blocks correctly. The private key can be stored in different backends, such as:

- **os backend**
- **file backend**
- **pass backend**
- **kwallet backend**
- **test backend**
- **memory backend**

For more details, visit the [Cosmos documentation on keyrings](https://docs.cosmos.network/v0.47/user/run-node/keyring).

```bash
export SIDED_KEYRING_BACKEND=os
```

#### **Adding Keys to the Keyring** 🔑

Generate a new keypair with:
```bash
sided keys add test --key-type="segwit"
```
This command creates a 24-word mnemonic phrase and saves it to the relevant backend. Make sure to store the mnemonic phrase securely! 

You can query your local keys with:
```bash
sided keys list
```

### **Initialize the Node** 🛠️

Before running the node, initialize the blockchain and its genesis file:
```bash
sided init <MY_SIDE_VALIDATOR> --chain-id=grimoria-testnet-1
```
This command creates all the necessary configuration files for your node in the `~/.side` directory. You can change the location of this folder by using the `--home` flag.

### **Configuring the Node Using `app.toml` and `config.toml`** ⚙️

The Cosmos SDK generates two configuration files:

- **config.toml:** Used to configure CometBFT. Refer to [CometBFT's documentation](https://docs.cometbft.com/) for more details.
- **app.toml:** Used to configure your app, including state pruning strategies, telemetry, gRPC, and REST server configurations.

Both files are heavily commented, so refer to them directly to tweak your node.

### **Download the Genesis File** 🌱

Download the genesis file and replace your local one:
```bash
wget https://raw.githubusercontent.com/sideprotocol/testnet/main/grimoria-testnet-1/genesis.json -O $HOME/.side/config/genesis.json
```

### **Adding Seeds and Persistent Peers** 🌐

Edit the `config.toml` file to add seeds and persistent peers:
```bash
cd $HOME/.side/config
nano config.toml
```
Modify the seed and persistent node configuration using the details provided in the file:
```bash
persistent_peers = "6bef0693d7a31fed473b95123ad19b544b414093@202.182.119.24:26656,44f8009ed91fddd7ee51117482ede20fb4f33e78@149.28.156.79:26656"
```

### **Setting Up Minimum Gas Prices** ⛽

Open `app.toml` and set the `minimum-gas-prices` to define the minimum gas prices your validator node will accept:
```bash
minimum-gas-prices = "0.005uside"
```

### **Start Node and Sync** 🔄

Reset the state and start the node:
```bash
sided tendermint unsafe-reset-all
sided start
```

🎉 **Congratulations! Your node is now set up and running!** 🎉
