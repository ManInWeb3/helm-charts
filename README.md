# blockchain-nodes

This repository contains the `blockchain-node` Helm chart, which can generate Kubernetes manifests to deploy and expose a blockchain node using Ingress.

## Motivation to Use a Common Helm Chart for All Blockchain Nodes

**Pros:**
  - **More DRY Infra as Code:** All blockchain nodes share a significant amount of configuration, which is generalized in this Helm chart.
  - **Easier Infra as Code Development:** Using the same Helm chart for all nodes allows changes to be made in one place to bring new features or fix bugs for all nodes.
  - **Unified Code and Naming Conventions:** All nodes follow the same code and naming conventions specified in the Helm chart, resulting in a more uniform infrastructure that is easier to read and develop.

**Cons:**
  - **Increased Configuration Items:** Due to generalization, nodes' configurations include more items, resulting in more lines of code.

## Structure of the Repository

The Helm chart includes templates for the following objects:
  - **StatefulSet:** Deploys blockchain node containers. Init containers can be used to download snapshots, initialize data, etc. It will also automatically add a sidecar container with health probes to check the node's status.
  - **Services:** Generated for each [container's port](https://github.com/ManInWeb3/blockchain-nodes/blob/main/node-axelar-dojo1.yaml#L114).
  - **Ingresses:** Exposes ports to the outside world; see [example](https://github.com/ManInWeb3/blockchain-nodes/blob/main/node-axelar-dojo1.yaml#L122).
  - Use [additionalManifests](https://github.com/ManInWeb3/blockchain-nodes/blob/main/node-axelar-dojo1.yaml#L170) to provision any manifests, e.g., configmaps, podScrappers, etc.

Example node configurations can be found in the following files:
  - node-axelar-dojo1.yaml
  - node-base-mainnet-archival.yaml
  - node-pokt-mainnet.yaml
  - node-polygon-mainnet.yaml
  - node-solana-mainnet.yaml

## How to:

### Generate Kubernetes Manifests of a Node with the Helm Chart

To generate manifests of a node described in `node-solana-mainnet.yaml`, run the following command:
```bash
helm template \
  solana \
  helm/blockchain-node \
  -f node-solana-mainnet.yaml
```
You will see the generated manifests in the output of the command.

### Deploy a Node with the Helm Chart
To deploy a node, install the blockchain-node Helm chart and pass an additional value file for the required node.

For example, to deploy an Axelar node configured in node-axelar-dojo1.yaml config file:

```bash
helm install \
  axelar \
  helm/blockchain-node \
  -f node-axelar-dojo1.yaml
```