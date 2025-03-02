# HPDIC MOD Fall 2022

## Installation

- `sudo apt install docker.io`
- `sudo apt install docker-compose`
- `sudo apt install golang`
- `sudo apt install npm`
- `npm install node`
- Download a local copy of https://github.com/hpdic/fabric/blob/main/scripts/bootstrap.sh
- `chmod +x bootstrap.sh`
- `sudo ./bootstrap.sh`
- `export PATH=$HOME/hyfa/fabric-samples/bin:$PATH`

## Test Basic Network

- `cd /home/cc/hyfa/fabric-samples/test-network`
- `sudo ./network.sh up`
- `sudo ./network.sh down`

## Test Channels

```bash
export PATH=$PATH:$(realpath ../bin)
export FABRIC_CFG_PATH=$(realpath ../config)
export $(./setOrgEnv.sh Org2 | xargs)

./setOrgEnv.sh
./network.sh up createChannel -ca -c mychannel -s couchdb
peer channel list
```

## Test Chaincode

```bash
# For simple installation
peer chaincode install -n sacc -v 1.0 -p ../chaincode/sacc/  # DFZ: install the sample sacc chaincode
peer chaincode list --installed

# For full deployment, refer to: https://github.com/hpdic/fabric-samples/tree/main/asset-transfer-basic
```

# Running the test network

You can use the `./network.sh` script to stand up a simple Fabric test network. The test network has two peer organizations with one peer each and a single node raft ordering service. You can also use the `./network.sh` script to create channels and deploy chaincode. For more information, see [Using the Fabric test network](https://hyperledger-fabric.readthedocs.io/en/latest/test_network.html). The test network is being introduced in Fabric v2.0 as the long term replacement for the `first-network` sample.

Before you can deploy the test network, you need to follow the instructions to [Install the Samples, Binaries and Docker Images](https://hyperledger-fabric.readthedocs.io/en/latest/install.html) in the Hyperledger Fabric documentation.

## Using the Peer commands

The `setOrgEnv.sh` script can be used to set up the environment variables for the organizations, this will help to be able to use the `peer` commands directly.

First, ensure that the peer binaries are on your path, and the Fabric Config path is set assuming that you're in the `test-network` directory.

```bash
 export PATH=$PATH:$(realpath ../bin)
 export FABRIC_CFG_PATH=$(realpath ../config)
```

You can then set up the environment variables for each organization. The `./setOrgEnv.sh` command is designed to be run as follows.

```bash
export $(./setOrgEnv.sh Org2 | xargs)
```

(Note bash v4 is required for the scripts.)

You will now be able to run the `peer` commands in the context of Org2. If a different command prompt, you can run the same command with Org1 instead.
The `setOrgEnv` script outputs a series of `<name>=<value>` strings. These can then be fed into the export command for your current shell.

## Chaincode-as-a-service

To learn more about how to use the improvements to the Chaincode-as-a-service please see this [tutorial](./test-network/../CHAINCODE_AS_A_SERVICE_TUTORIAL.md). It is expected that this will move to augment the tutorial in the [Hyperledger Fabric ReadTheDocs](https://hyperledger-fabric.readthedocs.io/en/release-2.4/cc_service.html)


## Podman

*Note - podman support should be considered experimental. There are issues with volume mounting on MacOS that prevent this working. If wish to use podman a LinuxVM is suggested.*

A copy of the `install_fabric.sh` script is in the `test-network` directory. This has been enhanced to support a `podman` argument; if used it will use the `podman` command to pull down images and tag them rather than docker. The images are the same, just pulled differently

The `network.sh` script has been enhanced so that it can use `podman` and `podman-compose` instead of docker. Ensure that `CONTAINER_CLI` is set as below when running `network.sh` script. 

```bash
CONTAINER_CLI=podman ./network.sh up
````

As there is no Docker-Daemon when using podman, only the `./network.sh deployCCAAS` command will work.




