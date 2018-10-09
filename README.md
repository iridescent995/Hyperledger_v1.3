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
