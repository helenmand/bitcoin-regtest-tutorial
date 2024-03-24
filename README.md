<div id="top"></div>

<br />
<div align="center">
  <h1 align="center">Bitcoin Regtest Tutorial (MacOS)</h1>
  <p align="center">by Mandana Eleni</p>
</div>

# Table of Contents
- [Set up Bitcoin Core on MacOS](#SetUp)
- [Run bitcoind regtest](#run)
- [Create a Wallet](#wallet)
- [Create a new Address](#newAddr)
- [List the addresses you have in your wallet](#listRecAdd)
- [View the private key of an Address](#pkadd)
- [Verifying an Address](#addval)
- [Check Account Balance](#accbal)
- [Check the current Block Count in the Blockchain](#cblockc)
- [Check the hash of the latest Block in the Blockchain](#latestbhash)
- [Generate a block](#genblock)
- [Get Block Info](#blockinfo)
- [Get Transaction Info](#txinfo)
- [See your UTXOs](#utx)
- [Send 1 BTC to another address](#sendBTC)
- [Check mempool](#memp)
- [How to confirm a Transaction](#conf)
- [How to confirm a Backup a Wallet and Import it again](#bckupW)
- [How to password protect your wallet](#ppwallet)
- [How to Kill Bitcoin Deamon](#killbd)

<div id="SetUp"></div>

## Setting up Bitcoin Core on macOS

Download bitcoin core, `sha256sums` and `sha256sums.asc` files from the latest releases: [https://github.com/bitcoin/bitcoin/releases](https://github.com/bitcoin/bitcoin/releases)

Unzip the arm file and check the checksum:

    cd Downloads/
    shasum -a 256 --check SHA256SUMS
    cd bitcoin-YOUR_VERSION/bin
    ./bitcoind -version # IT WON'T RUN

Before solving this, open another terminal and:
    
    cd /Users/YOUR_USERNAME/Library/Application Support/Bitcoin
    touch bitcoin.conf

Add these in the `.conf` file: 


    regtest=1
    server=1
    deprecatedrpc=create_bdb # this one is going to be deprecated soon-ish
    [regtest]
    maxtxfee=0.01
    fallbackfee=0.001


In case the bitcoin folder does not exist just create it, you also access this path from the Desktop > Go > /Users/YOUR_USERNAME/Library/Application Support/

Now, to make it work you need to:
1. `codesign -v -d bitcoind`
2. `codesign -s bitcoind # codesign bitcoind`
3. `xattr -d com.apple.quarantine $(pwd)/bitcoind # remove it from quarantine`
4. `codesign -v -d bitcoin-cli # same process for every file you want to use in the bin folder`
5. `codesign -s bitcoin-cli # codesign bitcoin-cli`
6. `xattr -d com.apple.quarantine $(pwd)/bitcoincli`

<div id="run"></div>

## Run bitcoind regtest 

To run the bitcoin deamon on the regtets run the following command:

    ./bitcoind -regtest

<div id="wallet"></div>

## Create a Wallet
In order to create a wallet run:

    ./bitcoin-cli createwallet "walletname"

To create a <b>legacy</b> wallet:
    
    ./bitcoin-cli createwallet "wallet_name" false false pass true false
<div id="newAddr"></div>

## Create a new Address
To create a new address run:

    ./bitcoin-cli getnewaddress

To create a new legacy address:
    
    ./bitcoin-cli getnewaddress "" "legacy" 

<div id="listRecAdd"></div>

## List the addresses you have in your wallet
To list the addresses and include those with 0 confirmations, that have not received payments yet, run:

      ./bitcoin-cli listreceivedbyaddress 0 true

<div id="pkadd"></div>

## View the private key of an Address

To check the private key of a specific address, run: 

    ./bitcoin-cli dumpprivkey "YOUR_ADDRESS"

<div id="addval"></div>

## Verifying an Address

To check whether an address is valid, run:

    ./bitcoin-cli validateaddress "ADDRESS"

<div id="accbal"></div>

## Check Account Balance

To check the wallet (account) balance, run:

    ./bitcoin-cli getbalance 

<div id="cblockc"></div>

## Check the current Block Count in the Blockchain

To check the current block count in the blockchain, run:

    ./bitcoin-cli getblockcount

<div id="latestbhash"></div>

## Check the hash of the latest Block in the Blockchain

To get the hash of the latest (best) block in the blockchain, run:

    ./bitcoin-cli getbestblockhash

This is the hash of the genesis block (if you have not created any others)

<div id="genblock"></div>

## Generate a block

To generate 1 block to a specific address, run:

    ./bitcoin-cli generatetoaddress 1 "YOUR_ADDRESS"

<div id="blockinfo"></div>

## Get Block Info

Get the information of a specific block:

    ./bitcoin-cli getblock "BLOCK_HASH"

The output is this:

    {
      "hash": "26474d86f8f374bca9b433d4168fd1cdcb085743ba8a838427bdc3cdab484536",
      "confirmations": 1,
      "height": 1,
      "version": 536870912,
      "versionHex": "20000000",
      "merkleroot": "b9aafeb9a73e49c6fdd01bd50feed02a1e45eaf87281858f8ca32ae1c96f3293",
      "time": 1711262352,
      "mediantime": 1711262352,
      "nonce": 1,
      "bits": "207fffff",
      "difficulty": 4.656542373906925e-10,
      "chainwork": "0000000000000000000000000000000000000000000000000000000000000004",
      "nTx": 1,
      "previousblockhash": "0f9188f13cb7b2c71f2a335e3a4fc328bf5beb436012afca590b1a11466e2206",
      "strippedsize": 215,
      "size": 251,
      "weight": 896,
      "tx": [
        "b9aafeb9a73e49c6fdd01bd50feed02a1e45eaf87281858f8ca32ae1c96f3293"
      ]
    }

You can see the info of the previous block's hash, the merkle root hash, the number of transactions.


<div id="txinfo"></div>

## Get Transaction Info

To get the information for a specific transaction, run:

    ./bitcoin-cli gettransaction "TX_HASH"

Results:

    {
      "amount": 0.00000000,
      "confirmations": 1,
      "generated": true,
      "blockhash": "26474d86f8f374bca9b433d4168fd1cdcb085743ba8a838427bdc3cdab484536",
      "blockheight": 1,
      "blockindex": 0,
      "blocktime": 1711262352,
      "txid": "b9aafeb9a73e49c6fdd01bd50feed02a1e45eaf87281858f8ca32ae1c96f3293",
      "wtxid": "f0a43594466443d34fc5c039545217670ade583bc3dbeae6ecfcdddb69562d2c",
      "walletconflicts": [
      ],
      "time": 1711262352,
      "timereceived": 1711262352,
      "bip125-replaceable": "no",
      "details": [
        {
          "address": "mvvwgRxoEExSQiF43eVVbH9gxHkTNABjGY",
          "parent_descs": [
          ],
          "category": "immature",
          "amount": 50.00000000,
          "label": "",
          "vout": 0,
          "abandoned": false
        }
      ],
      "hex": "020000000001010000000000000000000000000000000000000000000000000000000000000000ffffffff025100ffffffff0200f2052a010000001976a914a9135b6daf98358dc5397b972075c99f639f6ff488ac0000000000000000266a24aa21a9ede2f61c3f71d1defd3fa999dfa36953755c690689799962b48bebd836974e8cf90120000000000000000000000000000000000000000000000000000000000000000000000000",
      "lastprocessedblock": {
        "hash": "26474d86f8f374bca9b433d4168fd1cdcb085743ba8a838427bdc3cdab484536",
        "height": 1
      }
    }

The amount is 0 because it is a coinbase transaction. It is immature because the blockchain hasn't progressed far enough to validate the transaction. That's why if you run 'getbalance,' you will see 0. To be able to spend your coins, you need to wait for your transaction to be mined, 100 blocks later."

<div id="utx"></div>

## See your UTXOs

To see your UTCOs, run:

    ./bitcoin-cli listunspent

<div id="sendBTC"></div>

## Send 1 BTC to another address

First, create a new address and then send to this address 1 bitcoin with:

    ./bitcoin-cli sendtoaddress "recipient_address" 1

IMPORTANT: you need to load your wallets every time you run the bitcoind. To do so:

    ./bitcoin-cli loadwallet "wallet_name"


<div id="memp"></div>

## Check mempool

To check the mempool, run:

    ./bitcoin-cli getmempoolinfo  


To check the info of the transaction in the mempool run:

    ./bitcoin-cli getmempoolentry 96ea823a008c32f9e6786530bbfe84669c2937944aaf99e7654dec18177901ba

There you can see the fees you paid etc.

<div id="conf"></div>

## How to confirm a Transaction

If you run the `gettransaction` command, you will notice that the confirmation number is 0.

To confirm a transaction, you need to generate a block to include it. To do so, run:

    ./bitcoin-cli -generate 5

Now, if you check again, the confirmation number is 5. 

<div id="bckupW"></div>

## How to confirm a Backup a Wallet and Import it again

To backup a wallet you can run this:

    ./bitcoin-cli backupwallet "/Users/YOUR_USERNAME/Library/Application Support/Bitcoin/regtest/wallets/backup.dat"

and to load it again you can run this:

    ./bitcoin-cli loadwallet backup.dat

<div id="ppwallet"></div>

## How to password protect your wallet

To password protect your wallet, run:

    ./bitcoin-cli encryptwallet "my pass phrase"

<div id="killbd"></div>

## How to Kill Bitcoin Deamon

When you're done, to stop the bitcoin deamon, run:

    pkill bitcoind

