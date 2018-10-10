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

../bin/configtxgen -profile OneOrgChannel -outputCreateChannelTx ./config/channel.tx -channelID channel

generate anchor peer transaction

../bin/configtxgen -profile OneOrgChannel -outputAnchorPeersUpdate ./config/SellerMSPanchors.tx -channelID channel -asOrg SellerMSP

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



