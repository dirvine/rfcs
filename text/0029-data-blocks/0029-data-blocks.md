- Feature Name: Data Blocks
- Status: proposed
- Type: new feature
- Related components: (data, routing, vaults)
- Start Date: 08-03-2016
- RFC PR: (leave this empty)
- Issue number: (leave this empty)

# Summary

Data blocks are a container that allows large blocks of data to be maintained. These blocks can be
validated by a node on a network, close to the data name, to contain valid data that was very likely (if not almost
guaranteed).to have been correctly stored onto the network.

# Motivation

In a fully decentralised network there are many problems to solve, two of these issues can be thought of
as:

1. How to handle transferring large amounts of data to replicant nodes on each churn event.

2. How to allow data to be republished in a secure manner.

Point 2 actually encompasses two large issues in itself. The ability to start a node and make it's data
available is obviously required where we have large amounts of data to maintain. Another large advantage
is the ability for such a network to recover from a full system outage (full network collapse, worldwide
power outage etc.).

Another very useful "side effect" of data republish is in network upgrades. As long as two versions of
nodes have the ability to accept and store such data then even incompatible upgrades may be an option, that
was not previously possible. This component requires some further research, but would appear to offer a
significant advantage.

# Detailed design

A data block can be defined as:

A list of data type names that the node had stored over at least one session XXXXXXXXXXXXXX. The name of such a container
is derived as `sha512(all names) xor NodeName`. Proof that this node was responsible for these chunks
involves several checks.

1. The claiming node must sign the list with the private key of the `data block`.
2. The `DataBlock.key()` must provide the public key of the claiming node.
3. The claiming nodes ID must be in the close_group of the data_block.
4. The list of data types must contain data types greater than the number currently held by the group
 ( This means each data type ID or SD must outnumber the size currently held by the group)
    -If list of data held is less than provided then these DataBlocks must accumulate as per normal quorum rules
    XXXXXXXXXX Ths is no use as gamers can create off line attacks here XXXXXXXXXXXXXXXX

On node start it will pick up this list after bootstrapping on the network and send it to the group
closest to the previous name of the vault (this is written to the previous data store)

##When the network is growing

The data_block entries must be within the current close group of the claimant node

##When the network has shrunk

The data_block entries are allowed to be outwith the current close_group.

##Struct definition

```rust

```

On receipt of a Refresh message that contains a DataBlock, the receiving node will confirm each item the list of data names and types (DataIdentifier) are in it's close_group.


# Drawbacks

In very small networks (less than approx 3000) network difficulty is a fluctuating number, this can probably not be prevented, but may
allow unwanted data or in fact prevent valid data from being refreshed.


# Alternatives

What other designs have been considered? What is the impact of not doing this?

# Unresolved questions

What parts of the design are still to be done?
