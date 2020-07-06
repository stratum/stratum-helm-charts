# Stratum

Provides a [Helm] chart for deploying Stratum on Kubernetes.

# Installation

## Open Network Linux
You first need to install a customized Open Network Linux that comes with Docker and kernel modules required by K8s on the switch.
A pre-built ONL image can be found [here](https://github.com/opennetworkinglab/OpenNetworkLinux/releases)

Note: The minimum version is 1.3.0

## Kubernetes

Once the ONL is installed, you will need to install K8s on top of it. This can be done manually or with help of Rancher.
This can be done manually or with help of Rancher. See [k8s official install document](https://kubernetes.io/docs/setup/production-environment/) for detail.

Note: The minimum version is 1.17.5
