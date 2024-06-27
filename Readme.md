# What does this package exist? ##

## What is EigenLayer ###
EigenLayer is a protocol on Ethereum that introduces restaking, allowing stakers to reuse their staked ETH or Liquid Staking Tokens (LST) to secure additional applications and earn rewards. It enables pooled security by letting stakers delegate their ETH to operators who run validation services for Actively Validated Services (AVSs). This approach reduces capital costs and enhances trust for decentralized services by leveraging Ethereum's shared security.

## Centralization issues for AVS Developers ###
In the context of Actively Validated Services (AVS) on the EigenLayer protocol, EigenLayer's sdk does currenctly does not support a consensus mechanism for operators to act as Aggregators for task. Instead AVS developers must build manual intervention or a centralized entities to manage task assignments, which can introduce points of failure and reduce trust in the network.

## How does EigenShift solve this? ###
The EigenShift wraps around the [raft consensus protocol](https://github.com/hashicorp/raft), enabling operators to act as Aggregators and provides a trustless, programmatic framework to allow develoeprs to define how task are created, processed and submitted on-chain. This package leverages Raft for leader election between operators to elect Aggregators, tailored to handle EigenLayer tasks and operator BLS signatures. This pakcage also provides a mechanism for AVS developers to design how their AVS rotates Aggregators, and ensures consistent, reliable task validation and submission, enhancing the robustness and security of AVS deployments.

Thanks to using Go generics this package only has 3 external dependencies and provides developers with a framework to define their custom operator consesus:

- EigenLayer's go sdk: https://github.com/Layr-Labs/eigensdk-go
- Go ethereum: https://github.com/ethereum/go-ethereum
- Raft Protocol Implementation: https://github.com/hashicorp/raft

## Example AVS using this protocol

### Price Oracle AVS Example

To demonstrate how to use this protocol we created a demo price oracle AVS that aggregates price feed data from multiple trusted on-chain oracle networks (such as chainlink) and allows other contracts to consume that aggregated price feed data.

The EigenShift protocol is used to manage the automatic and trustless rotation of operators acting as task Aggregators. 

Here's how it is integrated:

- Leader Election (Aggregator assignment) and Task Submission: The protocol utilizes Raft to elect a new leader for each task. Every 15 seconds, a task is generated by the current leader operator, and a new operator is elected as the leader upon task submission.

- Operator Setup: The operators are configured to automatically register and join the existing cluster of operators on startup, ensuring they can participate in the consensus protocol.

This ensures a robust, decentralized process for task aggregation in the AVS. Check it out here: 
[Price Oracle AVS Example](https://github.com/Robert-H-Leonard/price-oracle-avs)
