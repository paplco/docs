# Validator node

#### Requires [Go 1.12+](https://golang.org/dl/)

### Install Golang

Add the Golang PPA repository to get the latest version of Golang.

`sudo add-apt-repository ppa:longsleep/golang-backports`

After adding the PPA, update packages list using the below command.

`sudo apt-get update`

![](https://i.imgur.com/tVxJFqU.png)

Install the latest version of Golang and other required packages

`sudo apt-get install -y git golang-go make`

![](https://i.imgur.com/tMFJJqQ.png)

#### Setting up Golang environment variables

Export the following Go environment variables

`export GOROOT=/usr/lib/go`

`export GOPATH=$HOME/go`

`export GOBIN=$GOPATH/bin`

`export PATH=$PATH:$GOROOT/bin:$GOBIN`

You can also append the above lines to $HOME/.bashrc file and run the following command to reflect in current Terminal session

`source $HOME/.bashrc`

![](https://i.imgur.com/OOyXrJz.png)

### Installing _**ixo-cosmos**_

1. Download ixo-cosmos files from the official [Ixo-Cosmos](https://github.com/ixofoundation/ixo-cosmos)

   `git clone github.com/ixofoundation/ixo-blockchain`

2. Navigate to the `ixo-blockchain` folder

   `cd ixo-blockchain`

   Check out the production version.

   `git checkout v1.6.0`

   ![](https://i.imgur.com/5NgrFM3.png)

3. Run following command to install the ixo-blockchain

   `make install`

   ![](https://i.imgur.com/8UbRTNc.png)

### 

