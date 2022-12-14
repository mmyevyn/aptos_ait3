# Deploy the fullnode
Recommended to install fullnode on a separate server
## For running an fullnode i recommend the following:

- CPU: 4 cores 
- Memory: 8GB RAM

### 1. Update packages
```
sudo apt update && sudo apt upgrade -y
```
### 2. Install dependencies
```
sudo apt-get install jq unzip -y
```

### 3. Install docker
```
sudo apt-get install ca-certificates curl gnupg lsb-release wget -y
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io -y
```

### 4. Install docker compose
```
mkdir -p ~/.docker/cli-plugins/
curl -SL https://github.com/docker/compose/releases/download/v2.6.1/docker-compose-linux-x86_64 -o ~/.docker/cli-plugins/docker-compose
chmod +x ~/.docker/cli-plugins/docker-compose
sudo chown $USER /var/run/docker.sock
```

### 5. Install fullnode node
#### Create directory
```
mkdir ~/testnet && cd ~/testnet
```


### Download config files
```
wget -qO docker-compose.yaml https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/aptos-node/docker-compose-fullnode.yaml
wget -qO fullnode.yaml https://raw.githubusercontent.com/aptos-labs/aptos-core/main/docker/compose/aptos-node/fullnode.yaml
```

#### Edit `fullnode.yaml` file to update the IP address for Validator node
Open `fullnode.yaml` file in nano editor
```
nano fullnode.yaml
```

Change `<Validator IP Address>` to your validator IP
![image](https://user-images.githubusercontent.com/95987354/185751546-2dea9366-b901-4206-8f42-846a975fc653.png)
Press `Ctrl + O` then press `Ctrl + X` and `Enter` to save changes to file

#### Copy the `keys`, `genesis.blob` and `waypoint.txt` files from validator node into the same working directory on Fullnode machine
![image](https://user-images.githubusercontent.com/95987354/185796510-c12f53fa-468a-4bc1-8900-2c2e5f90a513.png)


### Run docker compose
```
docker compose up
```

## Useful commands
### Check fullnode node logs
```
docker logs -f testnet-fullnode-1 --tail 50
```

### Check sync status
```
curl 127.0.0.1:9101/metrics 2> /dev/null | grep aptos_state_sync_version | grep type
```

### Restart docker container
```
docker restart testnet-fullnode-1 
```

