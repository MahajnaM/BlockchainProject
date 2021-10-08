We use Markdown and Ubuntu 18: [link to Markdown](https://guides.github.com/features/mastering-markdown/#syntax)

<h1> Installation Instructions: </h1>

<h2> Prerequisites: </h2>

Assuming the $HOME variable points to your home directory <br>
Update the distribution: ```sudo apt-get -y update && sudo apt-get -y upgrade```  <br>
First install [Java SDK 11](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04). <br>
Then install [Python 3.7](https://linuxize.com/post/how-to-install-python-3-7-on-ubuntu-18-04/). In addition install the following packets: <br>
```pip3 install ruamel.yaml```

Install [NodeJS, version 12](https://computingforgeeks.com/how-to-install-nodejs-on-ubuntu-debian-linux-mint/) <br>
Install [the latest Docker](https://phoenixnap.com/kb/how-to-install-docker-on-ubuntu-18-04) <br>
Download and install Golang. This is 1.16.4 but you may get the latest one from [here](https://golang.org/dl/) <br>

```sudo apt-get -y update && sudo apt-get -y upgrade```  <br>
```wget https://dl.google.com/go/go1.16.4.linux-amd64.tar.gz``` <br>
```sudo tar -xvf go1.16.4.linux-amd64.tar.gz```<br>


Now your Go distribution resides in $HOME/go. <br>

Create the folder **gopath** you will out all your Go projects into: <br>
```mkdir -p $HOME/gopath/src/github.com/hyperledger``` <br>
```cd $HOME/gopath/src/github.com/hyperledger```   <br>

Install [Hyperledger Fabric Binaries and Samples](https://hyperledger-fabric.readthedocs.io/en/release-2.2/install.html#) into the above mentioned folder: <br>
``` curl -sSL https://bit.ly/2ysbOFE | bash -s ```

Configure the Go-related environment variables as follows: <br>

``` echo 'GOROOT=$HOME/go' >> ~/.bashrc ``` 	<br>
``` echo 'GOPATH=$HOME/gopath' >> ~/.bashrc >> ~/.bashrc ``` <br>
``` mkdir -p $GOROOT/bin && mkdir -p $GOPATH/bin``` <br> 
``` echo 'PATH=$PATH:$GOROOT/bin:$GOPATH/bin' >> ~/.bashrc ``` <br> 
``` echo 'PATH=$HOME/gopath/src/github.com/hyperledger/fabric-samples/bin:$PATH' >> ~/.bashrc```  <br>
``` source ~/.bashrc ```

<h3> Install Hyperledger Fabric from sources: </h3>

This section is optional and we will use it only in advanced stages when Hyperledger behavior will be altered. 

Change the directory into the Fabric projects:
``` cd $HOME/gopath/src/github.com/hyperledger ```

Download the source code as described [here](https://www.digitalocean.com/community/tutorials/how-to-install-java-with-apt-on-ubuntu-18-04): <br>
``` git clone https://github.com/hyperledger/fabric.git ```  <br>
``` cd $GOPATH/src/github.com/hyperledger/fabric ```   <br>

To rebuild the entire Hyperledger distribution from scratch, you have to run the commands: <br>
``` make dist-clean all ```  <br>
``` make clean ```  <br>
``` make build ```  <br>
``` make docker ```  <br>

After finishing the process run the command ``` docker images ``` and you will find the most up-to-date Docker images for Hyperledger. They are preceded by
the **hyperledger/** word, like **hyperledger/fabric-orderer**. These images will appear in the **images** section of a file **docker-compose.yaml**. that will be used in any setting representing a Hyperledger instance as a Docker Compose swarm.



<h2> Launch the prototype: </h2>

Clone the example repository into the folder and set up the envoronment: <br>
``` $HOME/gopath/src/github.com/hyperledger/fabric-samples``` <br>
``` git clone https://github.com/binunalex/hfconfig.git ```   <br>
``` cd hfconfig```   <br>
``` echo "export PROTPATH=$(pwd)" >>~/.bashrc```   <br>
``` source ~/.bashrc```   <br>


Then run the following commands: <br>
```echo "127.0.0.1 ca.org1.dredev.de" >> /etc/hosts``` <br>
```echo "127.0.0.1 orderer1.dredev.de" >> /etc/hosts``` <br>
```for i in {1..10}``` <br>
```do```<br>
	```echo "127.0.0.1 peer$i.org1.dredev.de" >> /etc/hosts``` <br> 
```done```<br>

This script associates the DNS names of all Hyperledger members (peers,orderers etc) with the local IP. The associations are published in the file [/etc/hosts](https://techpiezo.com/linux/etc-hosts-hosts-file-in-ubuntu-distribution/) which serves as the "DNS pool" of Ubuntu. That is, the entire Hyperledger runs locally. In the future we will set different IPs, making Hyperledger run on a cloud.

Now launch the prototype:  <br> 
  
``` . setContext 0 ``` <br> 
``` ./generateAll.sh```   <br> 

Wait for a while. Then run a client:  <br> 

``` cd HFClient```   <br> 
```./launchClient.sh```  <br> 

You will be running a few queries - modification and fetch.


	
