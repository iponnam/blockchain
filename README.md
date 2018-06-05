# BlockChain - Hyper Ledger Fabric
## ZOOKEEPER Version 3.4.12 
Current Stable Version of Zookeeper is 3.4.12 as of June 5 2018
zookeeper 3.4.12 is available [here](https://hub.docker.com/r/iponnam/zk/)
use `sudo docker pull iponnam/zk:3.4.12` to pull the image 

## KAFKA Version 1.1.0
Current Stable Version of KAFKA is 1.1.0 as of June 5 2018
KAFKA 1.1.0 docker image is available [here](https://hub.docker.com/r/iponnam/kafka/)
use `sudo docker pull iponnam/kafka:1.1.0` to pull the image
This image doesn't entrypoint/CMD relavant to start the kafka service. So, one need to create another Dockerfile to include kafka start up script with all neccesary properties.
