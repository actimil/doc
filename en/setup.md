#### 1.Required environments

To run the Mypher, following environment is needed.

* docker

To build the Mypher, following environments are needed in addtion to abobe.

* git
* cmake
* EOS
* C++ compiler(like clang)

#### 2.How to build
Mypher can be run from starting Docker image of it.  
Currently, there is no fixed docker image because of development phase.  
To create the docker image, it is necessary to do following procedures.

1. Download all source codes from GitHub
1. Build the smart contracts
1. Build the docker image

The details are as belows.

##### 2-1. Download the source codes

Firstly, you have to download the source codes to a directory you like using following command.

```
git clone https://github.com/mypher/mypher
```

##### 2-2. Build the smart contracts

Next, you have to generate the smart contracts files(abi, wasm) using following commands.

```
cd {project root}/smartcontract
mkdir build
cd build
cmake ..
make
```

##### 2-3. Build the docker image

Finally, you have to generate the docker image using following commands.

```
cd {project rooot}/docker/image
./make.sh
```

The docker image named `mypher` is generated if successfully completes.

#### 3.How to run

Currently, this project has prepared just test environment.  
The smart contracts is not deployed on the mainnet of EOS yet.  
The test environment can be run on local machine. it is structured from 3 docker containers and we access to one of them from the browser of host machine.

![](img/local_environment.png)

The name of each container is as below.  
* mypher_eosio
* mypher_myphersystem
* mypher_user

You can start the test environment from following procedures

##### 3-1.prepare the local net

Firstly, you have to create docker network and 3 containers.

```shell
cd {project root}/docker/
./prepare_netowork.sh # create docker network
./prepare_container.sh # create docker containers 
```

##### 3-2. Run the containers

Next, you should start each container from following procedures.

**mypher_eosio**  
This container corresponds to a peer of mainnet which is not related to Mypher.it includes the scripts for initializing EOS blockchain and generating EOS system account.  
Thereby it should be run firstly.
It can be started using following commands.

```shell
cd {project root}/docker/
./start.sh eosio
```

**mypher_myphersystem**  
This container corresponds to the peer of mainnet which is owned by this project.it includes the scripts for managing the system account of Mypher and deplying the smart contracts.  
Thereby it should be run after starting mypher_eosio. 
It can be started using following commands.

```shell
cd {project root}/docker/
./start.sh myphersystem
```

**mypher_user**  
This container corresponds to the peer of mainnet which is owned by Mypher user.  
It should be run after starting mypher_eosio and mypher_myphersystem.
It can be started using following commands.
 
```shell
cd {project root}/docker/
./start.sh user
```

#### 4.How to use

You can access the UI of Mypher from followning URL.

```
http://127.0.0.1:8800/index.html
```

![](img/screen_search.gif)

The test environment includes 3 users as below by default.  
* testuser1111
* testuser2222
* testuser3333

You can login with each user using followin menu (it is for developments)
* login with testuser1111
* login with testuser2222
* login with testuser3333
