# Proof of Authority Development Chain

## DiasproNet1 Blockchain POA Network

DiasproNet1 is a private testnet blockchain for ZBank. It will explore what blockchain technology can do for ZBank and their customers. The Proof of Authority (PoA) algorithm is typically used for private blockchain networks as it requires pre-approval of, or voting in of, the account addresses that can approve transactions (seal blocks).

**Environment Setup & Tools Required**
* Download **Go Ethereum** Tools version **Geth & Tools 1.9.7** from [Go Ethereum](https://geth.ethereum.org/downloads/).

* Download **MyCrypto** Desktop App from [MyCrypto](https://download.mycrypto.com/). MyCrypto is a free, open-source, client-side interface that allows you to interact directly with the blockchain.

* **Puppeth** (to generate the genesis block)

* Clique Proof of Authority Algorithm

* **Terminal** in Mac

## Launching the DiasproNet1 network

* **Step 1.** After downloading the toos archive **Geth & Tools 1.9.7** decompress and save the file named **geth-alltools-darwin-amd64-1.9.7-a718daa6.tar.gz** in your computer hardrive.

* Renaming it as *DiasproNet1*. 

* The tools saved here will be used in developing our blockchain

* **Step 2.** Generate two new nodes with new account addresses within the DiasproNet1 folder that will serve as our pre-approved sealer addresses. 

    > ./geth --datadir node1 account new
    > ./geth --datadir node2 account new

* **Step 3.** After choosing the password and record the public key of the two nodes.
    > * Sealer address Node1: 0xFEDC4E75751dd9709FFf094e4f90d531a1117b47
    > * Sealer address address Node2: x32B053D602Ad9F8ffadAA69a26aA24490aC1674A

* **Step 4.** Create a genesis block using **puppeth** by following these steps:

* 4.1   Open a terminal window, navigate to the **DiasproNet1** folder and type the following command:`./puppeth`
         > This should show the following prompt.
         
![Puppeth run](https://github.com/machkev/blockchain-proof-of-authority-development/tree/main/screenshots/run_puppeth.png).
          
4.2   Type in the name for the network, *"DiasproNet1"* and hit enter to move forward in the wizard.

* 4.3   Type `<2>`to pick the `<Configure new genesis option>`, then `1` to `Create new genesisfrom scratch`:

![Configuring Genesis](https://github.com/machkev/blockchain-proof-of-authority-development/tree/main/screenshots/configuring_genesis.png)

* 4.4   Choose the `Clique (Proof of Authority)` consensus algorithm.

* 4.5   Paste both account addresses from the first step one at a time into the list of accounts to seal.

* 4.6   Paste them again in the list of accounts to pre-fund. There are no block rewards in PoA, so you'll need to pre-fund.

* 4.7   Choose `no` for pre-funding the pre-compiled accounts (0x1 .. 0xff) with wei.

* 4.8   Specify your chain/network ID: `444` and then hit enter. 

* 4.9   You should see a success message and redirected to the main menu. Choose the `Manage existing genesis` option.

* 4.10  Export genesis configurations. This will fail to create two of the files, but you only need `networkname.json`.

* **Step 5.** Initialize each node with the `new networkname.json` with `geth`.

* 5.1   Using `geth`, initialize each node with the new `networkname.json`.
*   ./geth --datadir node1 init diaspronet1.json
*   ./geth --datadir node2 init diaspronet1.json

* **Step** 6. Now, the nodes can be used to begin mining blocks

* 6.1   Run the nodes in separate terminal windows with the commands.

    *   ./geth --datadir node1 --unlock "SEALER_ONE_ADDRESS" --mine --rpc --allow-insecure-unlock

    *   ./geth --datadir node2 --unlock "SEALER_TWO_ADDRESS" --mine --port 30304 --bootnodes "enode://SEALER_ONE_ENODE_ADDRESS@127.0.0.1:30303" --ipcdisable --allow-insecure-unlock

    * **NOTE1**: Type your password and hit enter - even if you can't see it visually!

    * **NOTE2**: Run the first node, unlock the account, enable mining, and the RPC flag. Only one node needs RPC enabled.

    * **NOTE3**: Set a different peer port for the second node and use the first node's `enode` address as the `bootnode` flag.

    * **NOTE4**: If nodes are note mining and get error saying waiting for signers, add `--syncmode full` at the end of both commands.

The private Proof of Authority blockchain should now be running  and you should see both nodes producing new blocks!

## Send a Transaction

With both nodes up and running, the blockchain can be added to MyCrypto for testing.

1.  Use the MyCrypto GUI wallet to connect to the node with the exposed RPC port.With both nodes up and running, the blockchain can be added to MyCrypto for testing. Set up a custom network, and include the chain ID, and use ETH as the currency.

    ![Set Up Custom Node](https://github.com/machkev/blockchain-proof-of-authority-development/tree/main/screenshots/setting_up_custome_node.png)
        
    *   Open the MyCrypto app, then click `<Change Network>` at the bottom left:

    *   Click "Add Custom Node", then add the custom network information that you set in the genesis.

    *   Make sure that you scroll down to choose Custom in the "Network" column to reveal more  options like Chain ID.

    *   Type ETH in the Currency box.

    *   In the Chain ID box, type the chain id you generated during genesis creation.(`444`)

    *   In the URL box type: http://127.0.0.1:8545.  This points to the default RPC port on your local machine.

    *   Finally, click `Save & Use Custom Node`.

2.  Import the keystore file from the `node1/keystore` directory into MyCrypto. This will import the private key.

3.  Send a transaction from the `node1` account to the `node2` account.

4.  Copy the transaction hash and paste it into the "TX Status" section of the app, or click "TX Status" in the popup.

    ![Transaction Status](https://github.com/machkev/blockchain-proof-of-authority-development/tree/main/screenshots/tx_status.png)

5.  You just created a blockchain and sent a transaction!

## Defintions ##
 
    * RPC :   The `--rpc` flag enables us to talk to our second node, which will allow us to use MyCrypto(or Metamask)to transact on our chain.

    * MINE:   The `--mine` flag tells the node to mine new blocks.

    * PORT:   For listening to communication. Since the first node's sync port already took up 30320, we need to change this one to 30321 using `--port`.

    * BOOTNODES : The `--bootnodes` flag allows you to pass the network info needed to find other nodes in the blockchain. This will allow us to connect both of our nodes.

    * ENODE:  We will need this address to tell the second node where to find the first node.

    * MINERTHREADS:   The `--minerthreads` flag tells `geth` how many CPU threads, or "workers" to use during mining. Since our difficulty is low, we can set it to 1.

    * SYNCMODE FULL : Synchronizes a full node starting at genesis, verifying all blocks and executing all transactions.

    * BLOCKTIME = 5 seconds

    * Chain ID = 444

    * NetworkID = Protects a node from connecting to the nodes that are synchronizing with other networks. 


















 





























