# Deploying Nucypher (worker/staker) to Azure Cloud


### Instances Provisioning Playbooks for Azure

If you have Ansible setup to run playbooks against the Azure resource API then you can run the `deploy_nucypher_azure_infra.yml`


### Setting up your Ubuntu environment for running Ansible Azure

You have 3 options for using Ansible to deploy your infrastructure

1. Utilize the "cloud shell" within the Azure portal which comes pre-installed with Ansible and your credentials.
2. Use your own copy of Ansible and install the Azure module (through pip)
3. Setup your own deployment machine on Ubuntu to run playbooks and deploy workers.

Option 1 is ready to go, use the play book `deploy_nucypher_azure_infra.yml` followed by the playbooks in the /worker/ folder

For options 2 you will need Ansible (Azure module) installed on your local host (documentation [here](https://docs.ansible.com/ansible/latest/scenario_guides/guide_azure.html)).

For option 3 I've included the following steps below to setup a vanilla Ubuntu node to run Ansible (w/ Azure module), geth, and everything you need to deploy the Ansible playbooks for your Nucypher staker/workers.

(Instructions valid for Canonical Ubuntu 16.04 and 18.04)


#### Install python, pip
```
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install -y software-properties-common
sudo apt-get install -y python3.6
sudo apt-get install -y python-pip 
```

#### Install Ansible (w/ Azure module)
```
pip install --upgrade pip
pip install 'ansible[azure]'
pip install --upgrade 'ansible[azure]'
```

#### Export environment variables (Azure credentials)
```
export AZURE_CLIENT_ID=''
export AZURE_SECRET=''
export AZURE_SUBSCRIPTION_ID=''
export AZURE_TENANT=''
```

#### Create 2GB swap file (for local geth instance)
```
sudo fallocate -l 2G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
sudo cp /etc/fstab /etc/fstab.bak
echo '/swapfile none swap sw 0 0' | sudo tee -a /etc/fstab
```

#### Install local geth
```
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install -y ethereum
```

#### Run geth (goerli testnet, remove for mainnet)
```
nohup geth --goerli --syncmode fast --cache 1024 &
```
