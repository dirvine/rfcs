- Feature Name: Upgrades in SAFE
- Type new feature
- Related components safe_vault safe_nfs
- Start Date: 18-10-2015
- RFC PR: 
- Issue number: 

# Summary

Decentralised network upgrades are extremely difficult. These must be able to happen over a (short) 
period of time and muct allow at least two versions of the network nodes to be running and active
at any time. As a code update will require a node to restart then it also must mean that this cannot 
happen at a single point in time as the whole network would lose it's non-persistent state. This would
create a situation much like Skype several years back when a huge number of nodes (win98) all updated
at the same time. 

This is a reason to be aware of such issues and also to be aware that automated update systems (such
as yum, apt etc.) can create havoc with distributed systems by upgrading too many systems at a 
single time. 

There are several factors to consider in designing an upgrade mechanism for a decentralised network:

1. Nodes should understand, at minimum, the current and last version and be able to run with mixed
nodes.

2. Network state must persist and also be understood by old and new nodes alike. Transformation
of state to new code must not lose any data. 

3. All upgrades must be remote. 

4. It is not possible to remote login and manually upgrade, all upgrades will be automatic. 

5. Upgrade urgency, as only two versions will operate at one time there must be a mechanism where 
nodes with older than current - 1 version can "recover" their status as nodes by upgrading to 
`current`

##Further considerations

As an autonomous network that contains volotile network state (i.e. state vanishes with power) there is a need for extra care in ensuring network stability and size. A vast reduction in size would mean greater potential for data (state) loss. 

Wire protocol or message versioning would appear to be of very little use in the overall network as it does not answer the deeper questions of internal node objects, such as `state` containers, consensus algorithms etc. All of these add significant complexity if a simple approach is taken in upgrade versioning. Thinking deeper almost every code decision and object would require a seperate if statement or similar to decide which code branch to take. 

In routing for instance where there is the notion of group based consensus that relies on address proximity and also signing keys the impact of a change in encryption algorithm would be catastrophic. Also the consensus would be particulalry complex where not only a quorum would be difficult to code, but would contain messages of different types (currunt andf currnt -1 versions) whcih when serialised will not be equal, from a security standpoint as a current - 1 node is this version 2 data or in fact an attack? 

The next consideration is the inability to republish data at the moment as the network would depend on a client signature (and payment) for a Put or indeed a network group that can Put with the correct consensus. 

##Initial thoughts/brainstorming ideas

1.  Ensure nodes can restart as the same address by allowing them to keep a safecoin or similar signed structured data element. Such an element could be checked easily for correctness and if it contained a public key then the node can use it's stored private key to regain it's position in the network. If all nodes in a group did this the data would be republished `in place`
2.  Run two nodes in parallel, where one is `current` and one is `current -1` while there are any `current - 1` nodes in the groups. These nodes woudl share the private key via secured IPC and both read and write messages in their native format silently dropping those it did not understand.
3.  ......

# Motivation

Decentralised or autonomous networks cannot stop, restart or otherwise be controlled as a single 
entity, to do so would cease the network and lose state (information). As a complex system will
undoubtedly require to improve code, algorithms and add features then an upgrade system is essential.


# Detailed design

This is the bulk of the RFC. Explain the design in enough detail for somebody familiar
with the network to understand, and for somebody familiar with the code practices to implement.
This should get into specifics and corner-cases, and include examples of how the feature is used.

# Drawbacks

Why should we *not* do this?

# Alternatives

What other designs have been considered? What is the impact of not doing this?

# Unresolved questions

What parts of the design are still to be done?
