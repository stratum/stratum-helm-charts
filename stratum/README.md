# Stratum

Provides a [Helm](https://helm.sh) chart for deploying Stratum on Kubernetes.

# Installation

## Open Network Linux
You first need to install a customized Open Network Linux that comes with Docker and kernel modules required by K8s on the switch.
A pre-built ONL image can be found [here](https://github.com/opennetworkinglab/OpenNetworkLinux/releases)

Note: The minimum version is 1.3.0

## Kubernetes

Once the ONL is installed, you will need to install K8s on top of it. This can be done manually or with help of Rancher.
This can be done manually or with help of Rancher. See [k8s official install document](https://kubernetes.io/docs/setup/production-environment/) for detail.

Note: The minimum version is 1.17.5

## Provide Chassis Config

There are three options to provide chassis config. The order is described as follows:

### Option 1: Fetch chassis config from git repo

Please refer to the comment in `values.yaml` to learn more about how to configure the git repo.
The chassis config in the git repo should follow the naming convention: `<HOSTNAME>-chassis-config.pb.txt` such that the script can download the correct chassis config for each switch.

### Option 2: Upload chassis config to each switch

If the git repo config mentioned in option 1 is not supplied, the script will fallback and try to use a local chassis config.
You can manually upload chassis config to `/config/chassis_config.pb.txt` on the switch local filesystem.

NOTE: if both remote and local chassis configs are provided, the script will rename the local one to `chassis_config.pb.txt.old` and overwrite the `chassis_config.pb.txt` with the remote one.

### Option 3: Default chassis config

If none of the above is available, the script will use the default chassis config, which enables all ports with their maximum speed.
