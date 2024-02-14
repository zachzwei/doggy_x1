# Setting up an X1 Validator Node

Here are the steps for setting up your X1 TestNet validator. Thank you https://github.com/JozefJarosciak for sharing a step by step.

## Initial Setup
###
Type the following commands on your Terminal with root access.
```
cd
apt-get update && apt-get upgrade -y
```
```
apt-get install python3 python3-pip git nano wget jq tmux moreutils wine htop sudo vim make -y
```
```
sudo pip install passlib requests tqdm argon2_cffi web3==6.11.1
```
```
sudo git clone --branch x1 https://github.com/FairCrypto/go-x1 && sudo wget https://go.dev/dl/go1.21.4.linux-amd64.tar.gz && sudo tar -C /usr/local -xzf go1.21.4.linux-amd64.tar.gz && export PATH=$PATH:/usr/local/go/bin && source ~/.profile && source ~/.bashrc
```
```
cd && chmod -R 777 go-x1 && cd go-x1 && go mod tidy && make x1 && sudo cp build/x1 /usr/local/bin
```
## Downloading Chaindata Snapshot
###
This will get a snapshot of the chaindat. It will help your node to get synced much faster.
```
wget --no-check-certificate https://xenblocks.io/snapshots/chaindata1715.pruned.tar
```
```
tar -xvf chaindata1715.pruned.tar -C /root/.x1/
```
Once the chaindata is extracted to the X1 folder, you can proceed to the next steps.

#### Read-Only Node
At this point, you can run a read-only node by typing in the following command.
```
x1 --testnet --syncmode snap --xenblocks-endpoint ws://xenblocks.io:6668
```
You can continue setting up your validator node once you have your XN tokens that will be staked.

## Setting up X1 Validator

### Staking XN
####
Generate a validator key
```
x1 validator new
```
Take note of the pubkey and do a backup.
```
cp -r ~/.x1/keystore ~/.
```

Go to https://explorer.x1-testnet.xen.network/address/0xFC00FACE00000000000000000000000000000000/write-contract#address-tabs 
At the `createValidator` section, put your pubkey and indicate the amount on XN tokens to stake.
<img width="527" alt="image" src="https://github.com/zachzwei/doggy_x1/assets/35627271/2f2d4af6-a220-42cd-9672-807e4c1ca597">
Click on `Write` and confirm the transaction on Metamask.
Once its done, go to https://explorer.x1-testnet.xen.network/address/0xFC00FACE00000000000000000000000000000000/read-contract#address-tabs
At the `getValidatorID` section, put your address and click on `Query` to get your validator id.
<img width="375" alt="image" src="https://github.com/zachzwei/doggy_x1/assets/35627271/45cbf5e9-ec42-4647-b2cb-cf945ba099c1">


### Start X1 Validator
Type the following commands on your Terminal with root access.
```
cd
cd go-x1
make x1
sudo cp build/x1 /usr/local/bin
```
Run your validator by typing in the command below, indicating your validator id and pubkey
```
x1 --testnet --validator.id 69 --validator.pubkey 0xc0046........e --xenblocks-endpoint ws://xenblocks.io:6668 --gcmode full --syncmode snap
```



