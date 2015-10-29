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
