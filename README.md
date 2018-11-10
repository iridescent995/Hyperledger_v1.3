# Hyperledger_v1.3

config docker machine proxy : https://docs.docker.com/config/daemon/systemd/#httphttps-proxy

generate crytogen
```
../bin/cryptogen generate --config=./crypto-config.yaml
```

generate genesis block for orderer
```
../bin/configtxgen -profile OneOrgOrdererGenesis -outputBlock ./config/genesis.block
```

generate channel configuration transaction
```
../bin/configtxgen -profile OneOrgChannel -outputCreateChannelTx ./config/channel.tx -channelID channel
```
generate anchor peer transaction
```
../bin/configtxgen -profile OneOrgChannel -outputAnchorPeersUpdate ./config/SellerMSPanchors.tx -channelID channel -asOrg SellerMSP
```
bring up network
```
docker-compose -f docker-compose.yml down

docker-compose -f docker-compose.yml up -d
```
Create the channel
```
docker exec -e "CORE_PEER_LOCALMSPID=SellerMSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/users/Admin@Seller.market.com/msp" peer0.Seller.market.com peer channel create -o orderer.market.com:7050 -c channel -f /etc/hyperledger/configtx/channel.tx
```
Join peer0.org1.example.com to the channel.
```
docker exec -e "CORE_PEER_LOCALMSPID=SellerMSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/users/Admin@Seller.market.com/msp" peer0.Seller.market.com peer channel join -b channel.block
```
Join peer1.org1.example.com to the channel.
```
docker exec -e "CORE_PEER_LOCALMSPID=SellerMSP" -e "CORE_PEER_MSPCONFIGPATH=/etc/hyperledger/msp/users/Admin@Seller.market.com/msp" peer1.Seller.market.com peer channel join -b channel.block
```

### CLI commands

login to cli
```
docker exec -it cli bash
```

install chaincode
```
peer chaincode install -n mycc -v 1.0 -l java -p /opt/gopath/src/github.com/chaincode_example02/java/
```

instantiate cc
```
peer chaincode instantiate -o orderer.market.com:7050 --tls --cafile /opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/ordererOrganizations/market.com/orderers/orderer.market.com/msp/tlscacerts/tlsca.market.com-cert.pem -C channel -n mycc -l java -v 1.0 -c '{"Args":["init","a", "100", "b","200"]}' -P "OR ('SellerMSP.peer')"
```
