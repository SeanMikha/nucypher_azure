# nucypher_azure
set of best practices, playbooks, and guides for deploying nucypher nodes on Azure


#### Instances Provisioning Playbooks for Azure

If you already have Ansible setup to run playbooks against the Azure resource API then you can run the `deploy_nucypher_azure_infra.yml`

Otherwise see below for step-by-step guide for setting up a brand new azure account for infrastructure deployment.


#### Setting up your Ubuntu environment for running Ansible Azure

You will need to have Ansible (Azure module) installed on your local host (documentation [here](https://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html).

You can also use the Azure cloud shell (within the Azure portal, which comes pre-installed with Ansible (w/ Azure module) as well as your credentials)

You will also need the following credentials set as environment variables to use Ansible (w/ Azure module):

```
AZURE_CLIENT_ID
AZURE_SECRET
AZURE_SUBSCRIPTION_ID
AZURE_TENANT
```

I've also included a quick set of steps below to setup a vanilla Ubuntu 16.04 to run Ansible (w/ Azure module) all that is needed are the 4 credentials above:

# install python 3 , pip 3
```
sudo apt-get install -y software-properties-common ; sudo add-apt-repository ppa:deadsnakes/ppa ; sudo apt-get update ; sudo apt-get install -y python3 python3-pip 
```
# install Ansible (w/ Azure module)
```
sudo python -m pip install --upgrade pip ; sudo python -m pip --version ; sudo python -m pip install 'ansible[azure]' ;sudo python -m pip install --upgrade 'ansible[azure]'
```
# export environment variables (Azure credentials)
```
export AZURE_CLIENT_ID=''
export AZURE_SECRET=''
export AZURE_SUBSCRIPTION_ID=''
export AZURE_TENANT=''
```
# Create 2GB swap file (for local geth instance)
```
sudo fallocate -l 2G /swapfile ; sudo chmod 600 /swapfile ; sudo mkswap /swapfile ; sudo swapon /swapfile ; sudo cp /etc/fstab /etc/fstab.bak ; echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```
# Install local geth
```
sudo add-apt-repository -y ppa:ethereum/ethereum ; sudo apt-get update ; sudo apt-get install -y ethereum
```
# Run geth (goerli testnet, remove for mainnet)
```
nohup geth --goerli --syncmode fast --cache 1024 &
```

