# Readme
___

## PREREQUISITE Installation Windows

* Follow this [guide](https://medium.com/hackernoon/hyperledger-fabric-installation-guide-74065855eca9#30f8)
    - [X] Curl `curl --help`
    - [X] Virtualization
    - [X] [Docker](https://hub.docker.com/editions/community/docker-ce-desktop-windows)
    - [X] Check Docker version cmd `docker --version`
    - [X] Check Docker Compose cmd `docker-compose --version`
    - [X] Check Dockercmd hellow world cmd `docker run hello-world`
    - [X] Install Golang [**Official Site**](https://golang.org/dl/)
    - [X] Golang, Test install `go version`
    - [X] Install Node and NPM
    - [X] Test Node `node -v`
    - [X] Test NPM `npm -v`
    - [X] Install Python 2.7 ?? 3.9 is already installed globally
    - [X] `windows-build-tools` globally npm,  [address concerns](#concerns), USE --> `npm install --global windows-build-tools`
    - [X] Windows Tools, grpc [address concerns](#concerns), USE --> `npm install --global grpc`
    - [X] Install git *Already did because I'm a badasss*
    - [X] Add to PATH `C:\Users\arlei\Documents\ASU\CSE598-BlockchainApplications\Project1\fabric-samples\bin`
    - [X] Run a sample, Hyper ledger Fabric

___
## Reference
### Docker
* Username: arleighbw pw: **what do you think :)**
* **USER MANUAL** follow -> [link](https://docs.docker.com/docker-for-windows/)
#### Concerns
* installation: sent to this link for linux installs
    * [link for linux stuff :|](https://docs.microsoft.com/en-us/windows/wsl/install-win10#step-4---download-the-linux-kernel-update-package)
___
### Go
**GO GUIDE** follow -> [LINK](https://golang.org/doc/tutorial/getting-started)
___
### Node and NPM
#### Concerns
* Script for VS tools didn't execute because of stupid anti-virus
  * **MAY NEED TO RERUN** if things aren't running correctly.
    * [X] address with **npm build tools above**

* Note from Slack channel on versions
    * Guys - I have hyper ledger 2.0.1 with following node and NPM versions...
      * Node version : v14.16.0
      * NPM version : 6.14.11
    * These are definitely not working .. From slack messages I see below versions ...
      * **Node 8.9.4** <--I used
      * **npm 5.5.1** <-- I used

#### ISSUES
* had to install [this](http://wiki.overbyte.eu/wiki/index.php/ICS_Download#Download_OpenSSL_Binaries_.28required_for_SSL-enabled_components.29) in C:\OpenSSL-Win64\lib
* had to install [libeay32.lib](https://github.com/ReadyTalk/win32/blob/master/msvc/lib/libeay32.lib) into `/lib` folder

```
npm config set python python27
```
```
npm config set msvs_version 2015
//originally undefined
```
___
### HyperLedger Test
From tutorial 
```
curl -sSL http://bit.ly/2ysbOFE | bash -s
cd fabric-samples/first-network
./byfn.sh up
./byfn down
```

* [USE THIS TUTORIAL](https://hyperledger-fabric.readthedocs.io/en/latest/test_network.html) as 'first-network' is being replaced by 'test-network'
* **ALSO USE THIS FOR PROJECT** specific to version

For latest version, <-- **This is what I'm using**
`curl -sSL http://bit.ly/2ysbOFE | bash -s`
For recommended version // used byfn.sh.. didn't use
`curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.0.1 1.4.6 0.4.18`

Network up and creating Channel (genesis block)
```
./network.sh down //remove any containers or artifacts from prev runs
./network.sh up

docker ps -a

./network.sh createChannel
./newtork.sh createChannel -c channel1
./network.h up createChannel
```

Deploying Chaincode
```
./network.sh deployCC -ccn basic -ccp ../asset-transfer-basic/chaincode-go -ccl go
```

#### FABRIC_CFG_PATH
points to this directory (C:\Users\arlei\Documents\ASU\CSE598-BlockchainApplications\Project1\fabric-samples\config).

Run command from ./fabric-samples/test-network/
```linux
export FABRIC_CFG_PATH=$PWD/../config/ 
```

#### Environment variables for Org1
```
export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051
```

Run the following command to initialize the ledger with assets:
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"InitLedger","Args":[]}'
```

Query
`peer chaincode query -C mychannel -n basic -c '{"Args":["GetAllAssets"]}'`

Change owner
```
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile "${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem" -C mychannel -n basic --peerAddresses localhost:7051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt" --peerAddresses localhost:9051 --tlsRootCertFiles "${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt" -c '{"function":"TransferAsset","Args":["asset6","Christopher"]}'
```

peer2
```
# Environment variables for Org2

export CORE_PEER_TLS_ENABLED=true
export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
export CORE_PEER_ADDRESS=localhost:9051
```

`peer chaincode query -C mychannel -n basic -c '{"Args":["ReadAsset","asset6"]}'`
___
### DBCouch
https://docs.couchdb.org/en/stable/api/database/find.html?highlight=%22%5C%24in%22#

```docker
docker pull couchdb
```

edit core.yaml at `FABRIC_CFG_PATH` ..`./fabric-sample/config/core.yaml`
```
stateDatabase: CouchDb
```
___
### TESTING your smart contracts on fabric network
```
# Install hyperledge and fabric-samples
# FOR REC VERSION curl -sSL https://bit.ly/2ysbOFE | bash -s -- 2.0.1 1.4.6 0.4.18
curl -sSL https://bit.ly/2ysbOFE | bash -s
```
```
# This will create a directory called fabric-samples under your current directory
# copy p1 into fabric-samples/
cp <project-file> <your_path>/fabric-samples/
# in p1 directory
npm install
```
```
# in fabric-samples/test-network 
# Start Test Network
./network.sh up -s couchdb 
# Create Channal choose one of the following
./network.sh createChannel
export PATH=${PWD}/../bin:${PWD}:$PATH
export FABRIC_CFG_PATH=$PWD/../config/
export CORE_PEER_TLS_ENABLED=true
# Org 1
export CORE_PEER_LOCALMSPID="Org1MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp
export CORE_PEER_ADDRESS=localhost:7051
# Package:
peer lifecycle chaincode package p1.tar.gz --path ../p1 --lang node --label p1
# Install
peer lifecycle chaincode install p1.tar.gz
# Verify Installed
peer lifecycle chaincode queryinstalled

```
```
export CC_PACKAGE_ID=<Package Id from output of previous command>
```
``` 
# Approve
peer lifecycle chaincode approveformyorg -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --channelID mychannel --name p1 --version 1.0 --package-id $CC_PACKAGE_ID --sequence 1 --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
# Check Commit Status
peer lifecycle chaincode checkcommitreadiness --channelID mychannel --name p1 --version 1.0 --sequence 1 --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem --output json
# Same for Org 2
export CORE_PEER_LOCALMSPID="Org2MSP"
export CORE_PEER_TLS_ROOTCERT_FILE=${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
export CORE_PEER_MSPCONFIGPATH=${PWD}/organizations/peerOrganizations/org2.example.com/users/Admin@org2.example.com/msp
export CORE_PEER_ADDRESS=localhost:9051
# Install
peer lifecycle chaincode install p1.tar.gz
# Verify Installed
peer lifecycle chaincode queryinstalled

export CC_PACKAGE_ID=<Package Id from output of previous command> 
```
```
# Approve
peer lifecycle chaincode approveformyorg -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --channelID mychannel --name p1 --version 1.0 --package-id $CC_PACKAGE_ID --sequence 1 --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem
# Finally
# Check Commit Status
peer lifecycle chaincode checkcommitreadiness --channelID mychannel --name p1 --version 1.0 --sequence 1 --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem --output json
# Commit
peer lifecycle chaincode commit -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --channelID mychannel --name p1 --version 1.0 --sequence 1 --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt
peer lifecycle chaincode querycommitted --channelID mychannel --name p1 --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem

```
```
# Run your chaincode
# CreateProductRecord
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n p1 --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"createProductRecord","Args":["1", "Test Product", "0", "TV"]}'

# CreateProductRecord
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n p1 --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"createProductRecord","Args":["2", "Test Product2", "0", "TV2"]}'


# getProductByKey
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n p1 --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"getProductByKey","Args":["1", "Test Product"]}'

# updateQuantity
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n p1 --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"updateQuantity","Args":["1", "Test Product", "0", "21"]}'

# unknownTransaction error
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n p1 --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"YouDontExist","Args":["1", "Test Product", "0", "21"]}'

# Query by Product Type
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n p1 --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"queryByProductType","Args":["TV"]}'

# Query by Mfg Date
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n p1 --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"queryByMfgdate","Args":["0"]}'

# Query by Type Dual
peer chaincode invoke -o localhost:7050 --ordererTLSHostnameOverride orderer.example.com --tls --cafile ${PWD}/organizations/ordererOrganizations/example.com/orderers/orderer.example.com/msp/tlscacerts/tlsca.example.com-cert.pem -C mychannel -n p1 --peerAddresses localhost:7051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org1.example.com/peers/peer0.org1.example.com/tls/ca.crt --peerAddresses localhost:9051 --tlsRootCertFiles ${PWD}/organizations/peerOrganizations/org2.example.com/peers/peer0.org2.example.com/tls/ca.crt -c '{"function":"queryByProduct_Type_Dual","Args":["TV", "TV2"]}'

```
```
# Shutdown and Cleanup
./network.sh down
```

___
## PROGRESS
### 03/26/2021
* init git repo
* install dependencies, and hyperledger
  
### 03/27/2021
* **DEAD**.. configured the test fabric network.
* finished simple .JS get, getter/setter, updates.
* **TODO**: configure couchDB
  * Query couchDB by ProductType, Mfgdate, Product
* **TODO**: UnknownTransaction Function

Also need to properly `.tar` code to be deployed..
* follow this [guide](https://hyperledger-fabric.readthedocs.io/en/release-2.2/deploy_chaincode.html#javascript)

### 04/04/2021
* All I needed to do was add the label to the `export CC_PACKAGE_ID`
* updating script to test other functions
* For Task 7, [LINK](https://hyperledger-fabric.readthedocs.io/en/release-2.2/developapps/transactionhandler.html)
* For Task 4-6, --> [LINK](https://docs.couchdb.org/en/latest/api/database/find.html)
  * For Task 6 
```json
The $or operator

The $or operator matches if any of the selectors in the array match. Below is an example used with an index on the field "year":

{
    "year": 1977,
    "$or": [
        { "director": "George Lucas" },
        { "director": "Steven Spielberg" }
    ]
}
```