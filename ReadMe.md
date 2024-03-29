
# Configure a Node to Connect to Arbitrum Mainnet

This is the list of activities carried out to create a new Node and configure it for the Arbitrum Mainnet.

## Steps
#### 1. Create a node in Nirvana Labs
a) Create a VPC "arbitrum-node-vpc"
![VPC](images/vpc.PNG)
b) Create a Key-Pair to use for SSH
    #ssh-keygen
c) Create a VM with the specification given in "https://docs.arbitrum.io/node-running/how-tos/running-a-full-node"
![VM Details](images/vm.PNG)
c) SSH into the node
    #ssh -i .ssh/nirvana-labs ubuntu@147.28.180.48
#### 2. Create Alchemy Application and get the "parent-chain.connection.url"
a) Register for Alchemy
b) Create an Application with "Ethereum" chain and "Mainnet" Network
c) View the API Key details and extract the parent chain connection url

![Parent Chain Connection URL](images/parent-chain-url.PNG)

#### 3. Configure Node for Arbitrum

a) Install Docker
    #sudo apt install docker.io

b) Create directory "/home/ubuntu/arbitrum" and provide full access

    #mkdir /home/ubuntu/arbitrum
    #chmod -Rf 777 /home/ubuntu/arbitrum
    #sudo usermod -aG docker $USER
Exit and log in again to affect the permissions

c) Get the L2 chain id 42170 for Nova from "https://docs.arbitrum.io/node-running/node-providers#rpc-endpoints"

d) Get the relevant Nitro data set from "https://snapshot.arbitrum.foundation/index.html". I used the "nitro-pruned.tar" dataset.

e) Run the docker command

    #docker run --rm -it -d -v /home/ubuntu/arbitrum:/home/user/.arbitrum -p 0.0.0.0:8547:8547 -p 0.0.0.0:8548:8548 offchainlabs/nitro-node:v2.2.5-a20a1c7 --parent-chain.connection.url https://eth-mainnet.g.alchemy.com/v2/w0pdvopp0fdgh8fPLRqA5m-fCMFgWfi2 --chain.id=42170 --http.api=net,web3,eth,debug --http.corsdomain=* --http.addr=0.0.0.0 --http.vhosts=* --init.url="https://snapshot.arbitrum.foundation/nova/nitro-pruned.tar"



### Verification

#docker logs -f containerid

This command will show the sync details with the network

Note: Please do not expose the "Parent chain connection url". Better to pass it to the env variable and refer to the variable in the docker command.

### Problems faced:

### 1. Finding the "parent-chain.connection.url"
    Initially refered to the "https://docs.arbitrum.io/node-running/node-providers#rpc-endpoints" and used "Arbitrum Nova" RPC URL "https://nova.arbitrum.io/rpc". It caused issues with the Chain Id values. After searching the Internet noted that Alchemy application can be used to get the parent connection url.
