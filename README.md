## Introduction

This project is a flexible testing infrastructure for Linux SMB Clients, powered by the Azure cloud platform. This project enhances the existing Buildbot framework by migrating it to Azure, offering a robust and scalable solution for testing Linux SMB Clients.

This repository provides the necessary code and resources for setting up a new instance of Buildbot and running tests using the platform.

**Architecture**

![Diagram](https://github.com/ShaanCoding/ReadME-Generator/assets/84325400/839b94cd-d16a-4afe-89c6-58e2dff24fe8)

## Inside the repository

![Inside the repository](https://github.com/ShaanCoding/ReadME-Generator/assets/84325400/fb0b8989-b025-4967-aaf9-ed73634308a4)

## Prerequisites

1. Operating System: Ubuntu
2. You should hava a Storage Account for creating fileshares.
3. Two fileshares, one named test and one named scratch.
4. A user assigned managed identity, which has the role of Contributor assigned to it.
5. You should have a shh key (public and private), these will be used for communication between Buildbot, Builder and Client Virtual Machines.
  * You can use your existing key or,
  * You can generate a new one using: `ssh-keygen`
6. You should also have an public key, private key pair connected to github.

## Installation - Spinning up a new instance of Buildbot and running tests

1. Fill the parameters in config.yaml file (these include)
  * env_name: Name of the env you want to give (example v1), will be appended to the VM names
  * region: region in which the azure resources will be created
  * resource_group: Name of your resource group
  * subscription_id: Id of your azure subscription
  * mi_name: Name of the User Assigned Managed Identity
  * object_id_mi: Object Id of the User Assigned Managed Identity
  * client_id: Client id of the User Assigned Managed Identity
  * share_url_scratch: url of the share fileshare
  * share_url_test: url of the test fileshare
  * ssh_public_key_path: Path of your ssh public key to be used (example ~/.ssh/your_key.pub)
  * ssh_private_key_path: Path of your ssh private key to be used (example ~/.ssh/your_key)
  * github_ssh_publick_key_path: Path of your ssh public key associated to github (example ~/.ssh/your_key_github.pub)
  * github_ssh_private_key_path: Path of your ssh private key associated to github (example ~/.ssh/your_key_github)

**Fields with default values**

  * buildbot_vm_name: Name that you want to give to the buildbot virtual machine (**default value** : buildbotvm)
  * builder_vm_name: Name that you want to give to the builder virtual machine (**default value** : buildervm)
  * client_vm_name: Name that you want to give to the client virtual machine (**default value** : clientvm)
  * buildbot_sku: Size of the Buildbot Virtual Machine (**default value** : Standard_D2s_v3)
  * builder_sku: Size of the Builder Virtual Machine (Should be enough to accomodate Kernel Build) (**default value** : Standard_D8s_v3)
  * client_sku: Size of the Client Virtual Machine (**default value** : Standard_D2s_v3)
  * image: the image of the Azure Virtual Machine to be used for builder,buildbot and client VMs (**default value** : Ubuntu2204)

2. Run the following to start the spinup
   `cd spinup` and `./smb-builbot-spinup.sh`
3. Open the url displayed after the spinup is complete.
4. Trigger builds by pushing the FORCE button on the WEB UI.

## Usage
flag | functionality
--- | ---
 -i, --installation-scripts | Run all installation scripts
 -c, --create-builder-vm | Run create-builder-vm script and subsequent setup scripts
 -s, --create-buildbot-vm  | Run create-buildbot-vm script and subsequent setup scripts
 -b, --setup-builder-vm | Run setup-builder-vm script and subsequent setup scripts
 -B, --setup-buildbot-vm  | Run only the setup-buildbot-vm script
 --help  | Show help information
