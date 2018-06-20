Pre-requisites software (Only for windows)

If you alrady have vagarnt and oracle virtual box installed, then kindly uninstall first, then install softwares from provided links

    Oracle Virtual Box ( https://download.virtualbox.org/virtualbox/5.1.34/VirtualBox-5.1.34-121010-Win.exe )
    Vagrant ( https://releases.hashicorp.com/vagrant/2.0.3/vagrant_2.0.3_x86_64.msi )
    Git and Git Bash ( https://git-scm.com/download/win )
    Install Window Powershell 3.0 or Upgrade to Window Powershell 3.0

Window 10 OS : Go to Control Panel --> Programs --> Click on "Turn window features ON or OFF" --> Disable "Hyper-V" feature

Window 7/10: Virtualization must be ENABLED in BIOS
Pre-requisites software (Only for Mac)

    https://hyperledger-fabric.readthedocs.io/en/release-1.1/prereqs.html

0: Learn Fabric

    git clone https://github.com/dparmar1/learnfabric.git

1: Fabric

    git clone https://github.com/hyperledger/fabric.git
    cd fabric
    git checkout release-1.1
    copy "Vagrantfile" from "learnfabric" folder and overwrite in "devenv" folder of fabric.
    cd ..
    git clone https://github.com/hyperledger/fabric-sdk-java.git
    cd fabric-sdk-java
    git checkout release-1.1

2: Initialize VM and Login into Ubuntu VM

    cd fabric/devenv
    vagrant up
    vagrant ssh
    cd /opt/gopath/src/github.com/hyperledger/fabric
    sudo usermod -a -G docker $USER
    exit
    vagrant ssh
    cd /opt/gopath/src/github.com/hyperledger/fabric
    make docker

3: Fabric CA (Need to clone within Virtual Box)

    cd /opt/gopath/src/github.com/hyperledger
    git clone https://github.com/hyperledger/fabric-ca.git
    cd fabric-ca
    make docker

4: Download Fabric Samples

    cd /opt/gopath/src/github.com/hyperledger
    git clone https://github.com/hyperledger/fabric-samples.git
    cd fabric-samples
    curl -sSL https://goo.gl/6wtTN5 | bash -s 1.1.0

5: Create Basic Fabric Network

    cd fabric-samples
    cd basic-network
    ./generate.sh [ DON't Execute this command]
    vi start.sh
    add keyword 'cli'
    Save start.sh file
    ./start.sh

6: Deploy and Test Chaincode (CLI)

    docker exec cli peer chaincode install -n mycc -v 1.0 -p github.com/chaincode_example02/go/
    docker exec cli peer chaincode instantiate -o orderer.example.com:7050 -C mychannel -n mycc -v 1.0 -c '{"Args":["init","a", "100", "b","200"]}' -P "OR ('Org1MSP.member')"
    docker exec cli peer chaincode invoke -C mychannel -n mycc -c '{"Args":["invoke", "a", "b", "50"]}'
    docker exec cli peer chaincode invoke -C mychannel -n mycc -c '{"Args":["query","a"]}'

7: Fabric Java SDK

    Install Java 1.8
    Install Maven ( https://www.mkyong.com/maven/how-to-install-maven-in-windows/ )
    Install Eclipse ( https://www.eclipse.org/downloads/download.php?file=/oomph/epp/oxygen/R2/eclipse-inst-win64.exe )
    git clone https://github.com/hyperledger/fabric-sdk-java.git
    cd fabric-sdk-java
    git checkout release-1.1
    cd ..

8: Deploy and Test Chaincode (Using Fabric Java SDK)

    Go to VM
    cd /opt/gopath/src/github.com/hyperledger/fabric/sdkintegration
    docker-compose down; rm -rf /var/hyperledger/*; docker-compose up --force-recreate -d
    Go to Host OS terminal and Keep your vagrant running
    cd fabric-sdk-java
    mvn eclipse:eclipse
    Open fabric-java-sdk as java project in eclipse
    Run End2endIT.java in eclipse

9: Deploy and Test Chaincode (Using Fabric Node SDK)

    git clone https://github.com/hyperledger/fabric-sdk-node.git
    cd fabric-sdk-node
    sudo apt-get update
    sudo npm install -g gulp
    sudo apt-get install build-essential curl git m4 ruby texinfo libbz2-dev libcurl4-openssl-dev libexpat-dev libncurses-dev zlib1g-dev
    sh -c "$(curl -fsSL https://raw.githubusercontent.com/Linuxbrew/install/master/install.sh)"
    export PATH="$HOME/.linuxbrew/bin:$PATH"

10: Reference Materials

    Hyperledger Fabric : https://hyperledger-fabric.readthedocs.io/en/release-1.1/
    Blockchain Badges: http://gbslearn.atlanta.ibm.com/iSPO/blockchain/index.html
    https://chainhero.io/2017/07/tutorial-build-blockchain-app/
