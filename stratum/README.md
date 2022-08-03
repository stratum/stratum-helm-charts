# Stratum

Provides a [Helm](https://helm.sh) chart for deploying Stratum on Kubernetes.

# Installation

## SONiC
You first need to install a customized SONiC that comes with additional platform support.

Pre-built SONiC installers can be found [here](https://github.com/stratum/sonic-base-image/releases)

To install SONiC on the switch, please follow [this installation guide](https://github.com/sonic-net/SONiC/wiki/Quick-Start#installation).

## Kubernetes

Once the SONiC is installed, you will need to install Kubernetes on top of it.

This can be done manually or with help of Rancher. See [k8s official install document](https://kubernetes.io/docs/setup/production-environment/) for detail.

Note: The minimum Kubernetes version is 1.17.5

## Provide Chassis Config

There are two options to provide chassis config. The order is described as follows:

### Option 1: Helm Chart value

Please refer to the chassis_config field in `values.yaml` to learn more how to configure the chassis_config.
Chassis_Config is a key-value object and the key must be the **Kubernetes node name**. you can get it via `kubectl get nodes`.


### Option 2: Default chassis config

If none of the above is available, the script will use the default chassis config, which enables all ports with their maximum speed.
