# Arisen Block Producer Documentation
Below are instructions on how to install a full Arisen Node and register as an Arisen block producer (BP) in order to become a candidate for producing blocks on the Arisen network:

## 1. One-Line Arisen Installer 
This one-line installer will install all Arisen dependencies on a specific operating system (auto-determined), while building all Arisen binaries and daemons. 

```
git clone https://github.com/arisenio/bp-installer.git && cd bp-installer && sh install.sh
```

## 2. Start ```awallet``` & ```aos``` 
Startup ```awallet``` in a ```screen```

```
screen
awallet --http-server-address 127.0.0.1:8889
```

**Exit Screen (CTRL + A + D)**

Then startup ```aos``` in a ```screen```
```
screen
```

```
aos --p2p-listen-endpoint 127.0.0.1:6620 --p2p-peer-address greatchain.arisen.network:6620 --plugin arisen::producer_plugin --plugin arisen::chain_api_plugin --plugin arisen::net_api_plugin --plugin wallet_api_plugin --plugin http_plugin --plugin arisen::bnet_plugin --plugin arisen::net_api_plugin
```

**Exit Screen (CTRL + A + D)**

## 3. Create Arisen Wallet
```
arisencli --wallet-url 127.0.0.1:8889 wallet create --to-console
```
This will create your Arisen wallet, where your block producer keys will be stored. These keys will be used to sign transactions on the Arisen blockchain. The above command will output a password that starts with "P". You need to save this in a safe place, as it will be used to unlock your Arisen Wallet (aWallet). The wallet will auto-lock, by default, every 15 seconds.

## 4. Create Block Producer Keys
```
arisencli create key --to-console
```
This will generate a keypair that will be registered as the "owner" keypair. Make sure to take record of this and keep it in a safe place. The public key related to this keypair is used in the next step. 

```
arisencli create key --to-console
```
This will generate another keypair that will be registered as the "active" keypair. Make sure to take record of this and keep it in a safe place. The public key related to this keypair is used in the next step.

```
arisencli create key --to-console
```
This will generate a third keypair that will be registered as the "bp" keypair. This is used to sign transactions and will be used as the key to register the block producer on the Arisen network. Make sure to take record of this and keep it in a safe place. The public key related to this keypair is used in the next step.

## 5. Create Block Producer User
Now it's time to create the block producer's account. This can also be a current account. So if you already have an Arisen account, there is no reason to create another one, as any account on the network can register as a block producer. If you already have a current account on the network and would like to register that account as a block producer you can do so in the next step and should skip this one. Otherwise, continue with this step below.

```
arisencli system newaccount $ORIGINATING_ACCOUNT $BP_USERNAME $BP_OWNER_PUBLIC_KEY $BP_ACTIVE_PUBLIC_KEY --buy-ram '1000000.0000 RSN' --stake-net '1000000.0000 RSN' --stake-cpu '1000000.0000 RSN'
```
**The variables above must be changed accordingly. Here is a description of the required variables:**
```$ORIGINATING_ACCOUNT``` - This is the account that is creating the BP account. For new block producers, the only way to create an account is if someone who already has an account in the community, creates an account for you or through an account registration service like [Arising](https://arising.io). 
```$BP_USERNAME``` - The username for the block producer.
```$BP_OWNER_PUBLIC_KEY``` - The ```owner``` public key of the block producer. You can easily generate keypairs with ```arisencli``` by running ```arisencli create key --to-console```.
```$BP_ACTIVE_PUBLIC_KEY``` - The ```active``` public key of the block producer. You can easily generate keypairs with ```arisencli``` by running ```arisencli create key --to-console```.

## 6. Register Block Producer 
Now the block producer has to be registered across the network, by running the command below:
```
arisencli system regproducer $BP_USERNAME $BP_PUBLIC_KEY $BP_URL $BP_LOCATION
```

**The variables above must be changed accordingly. Here is a description of the required variables:**
```$BP_USERNAME``` - The username of the block producer.
```$BP_PUBLIC_KEY``` - The ```bp``` public key of the block producer. You can easily generate keypairs with ```arisencli``` by running ```arisencli create key --to-console```. 
```$BP_URL``` - The URL to the website or blog of the block producer.
```$BP_LOCATION``` The location of the block producer (i.e. Dallas)

## 7. Restart ```aos```
Now ```aos``` must be restarted. Go to the ```screen``` where ```aos``` is running and shutdown the ```aos``` daemon.

Restart with the following command, using the above variables and settings:

```
aos --producer-name $BP_USERNAME --signature-provider $BP_PUBLIC_KEY=KEY:BP_PRIVATE_KEY --plugin arisen::producer_plugin --plugin arisen::chain_api_plugin --plugin arisen::http_plugin --plugin arisen::history_api_plugin --plugin arisen::history_plugin --plugin arisen::wallet_api_plugin --plugin arisen::bnet_plugin --plugin arisen::net_api_plugin --producer-name $BP_USERNAME --access-control-allow-origin=* --http-server-address 127.0.0.1:8888 --contracts-console --http-validate-host=false --verbose--http-errors --p2p-peer-address greatchain.arisen.network:6620 
```

## Developers
[Jared Rice Sr.](jared@dpeeps.com)
[NMH](nmh@dpeeps.com)
[CF](cfernandez@protonmail.com)

## Join The Community
[Join Telegram](https://t.me/arisenio)
[Follow On Steemit](https://steemit.com/@arisenio)
