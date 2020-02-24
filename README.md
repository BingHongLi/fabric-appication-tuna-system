# Hyperledger Fabric APP 搭建與Demo
手把手 教你搭建 Hyperledger fabric 的 APP

---

# 使用方式
## 下載專案
```
git clone https://github.com/BingHongLi/fabric-appication-tuna-system.git
```

## 下載 docker image
```
curl -sSL https://raw.githubusercontent.com/hyperledger/fabric/master/scripts/bootstrap.sh | bash -s 1.4.5
```

## 進入專案資料夾，並更新
```
cd fabric-appication-tuna-system  
git submodule update --init --recursive  
cd fabric-samples/basic-network  
./start.sh  
docker ps   
```

## 開啟客戶端
```
docker-compose up -d cli  
docker ps  
```

## 把整個資料夾，放入chaincode資料夾內
```
cp -r ../../lbh_tuna_demo ../chaincode/  
```

## 安裝chaincode在節點上
```
docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp" cli peer chaincode install -n lbh-tuna-demo -v 1.0 -p github.com/lbh_tuna_demo
```

## 在指定channel上初始化chaincode
```
docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp" cli peer chaincode instantiate -o orderer.example.com:7050 -C mychannel -n lbh-tuna-demo -v 1.0 -c '{"Args":[""]}' -P "OR ('Org1MSP.member','Org2MSP.member')"
```

## invoke chaincode
```
docker exec -e "CORE_PEER_LOCALMSPID=Org1MSP" -e "CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp" cli peer chaincode invoke -o orderer.example.com:7050 -C mychannel -n lbh-tuna-demo -c '{"function":"initLedger","Args":[""]}'
```

## Query chaincode
```
docker exec cli peer chaincode invoke -n lbh-tuna-demo -c '{"Args":["queryTuna","2"]}' -C mychannel
```

## 啟用api client 
```
cd ../..  
docker-compose up -d  
```

## api client 入口
http://localhost/login  

---

