# rosa-odf-netdev

This repo includes ansible scripts to setup different network topologies in AWS
for testing ODF provider / consumer configurations. It depends on the `amazon.aws`
ansible module

```
ansible-galaxy collection install amazon.aws
```

The aws module depends on boto3, so be sure it is installed as well.

```
pip3 install boto3
```

# Shared VPC

## Network setup
```
ansible-playbook shared-vpc.yaml
```

## Cluster creation

```
```

# Hub-and-Spoke VPCs with VPC Peering

## Network setup

```
ansible-playbook vpc-peering.yaml
```

## Cluster creation

```
```

# Hub-and-Spoke VPCs with VPC Transit Gateway

## Network setup

```
ansible-playbook vpc-transit.yaml
```

## Cluster creation

```
```

# Post

Subscribe to ocs-operator in provider cluster

Create `StorageCluster`

```
oc apply -f storagecluster.yaml
```

Create toolbox pod

```
oc patch OCSInitialization ocsinit -n openshift-storage --type json --patch '[{ "op": "replace", "path": "/spec/enableCephTools", "value": true }]'
```

Run export script

```
oc exec -n openshift-storage <tool-box-pod-name> -- python3 ceph-external-cluster-details-exporter.py --rbd-data-pool-name ocs-storagecluster-cephblockpool --run-as-user client.ocs > ceph_credentials.json
```
