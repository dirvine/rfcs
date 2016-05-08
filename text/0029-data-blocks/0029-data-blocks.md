- Feature Name: Data Blocks
- Status: proposed
- Type: new feature
- Related components: (data, routing, vaults)
- Start Date: 08-03-2016
- RFC PR: (leave this empty)
- Issue number: (leave this empty)

# Summary

Data blocks are a container that allows large blocks of data to be maintained. These blocks can be
validated by any node on a network to contain valid data that was very likely (if not almost
guaranteed).to have been correctly stored onto the network.

# Motivation

In a fully decentralised network there are many problems to solve, two of these issues can be thought of
as:

1. How to handle transferring large amounts of data to replicant nodes on each churn event.

2. How to allow data to be republished in a secure manner.

Point two actually encompasses two large issues in itself. The ability to start a node and make it's data
available is obviously required where we have large amounts of data to maintain. Another large advantage
is the ability for such a network to recover from a full system outage (full network collapse, worldwide
power outage etc.).

Another very useful "side effect" of data republish is in network upgrades. As long as two versions of
nodes have the ability to accept and store such data then even incompatible upgrades may be an option, that
was not previously possible. This component requires some further research, but would appear to offer a
significant advantage.


# Detailed design

This is the bulk of the RFC. Explain the design in enough detail for somebody familiar
with the network to understand, and for somebody familiar with the code practices to implement.
This should get into specifics and corner-cases, and include examples of how the feature is used.

A data block can be defined as:

A list of data type names that the node had stored over at least one session. The name of such a container
is derived as `sha512(all names) xor NodeName`. Proof that this node was responsible for these chunks
involves several checks.

1. The claiming node must sign the list with the private key of the `data block`.
2. The `DataBlock.key()` must provide the public key of the claiming node.
3.


# Drawbacks

Why should we *not* do this?

# Alternatives

What other designs have been considered? What is the impact of not doing this?

# Unresolved questions

What parts of the design are still to be done?
