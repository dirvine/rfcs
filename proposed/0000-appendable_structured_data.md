- Feature Name: Appendable Structured Data
- Type enhancement
- Related components maidsafe_client, routing, maidsafe_vault
- Start Date: 04-Oct-2015
- RFC PR: (leave this empty)
- Issue number: (leave this empty)

# Summary

This proposal is to allow structured data elements to allow append only elements. These may be 
limited to certain users or open to all. 

# Motivation

To make use of structured data as found in traditional file systems there is often a requirement
for a data type that allows append type options. As an example this would allow for network inboxes,
comment sections on data etc. as well as the usual append type data commonly known. 
This is not, however a substitute for log file type append data as the limitation on StructuredData 
size would of course kick in too quickly for such use. 


# Detailed design

This design adds another data element to the structured data type, this is a simple enumeration, 
defined as follows:

```rust
enum AppendRule {
white_list: Vec<crypto::sign::PublicKey> /// vec.empty == allow nobody (i.e. no append)
black_list: Vec<crypto::sign::PublicKey> /// vec.empty == allow anyone
}
```
This will be assisted with an implementation of `Default` for Append // to default to not append

The data element to hold the append data will require an indexed list of some description, this may
require such append data may require a group agreement on such an order. This is described in the 
next section and described in detail in the appropriately named RFC. In the meantime this will be
held as follows:

```rust
apppend_data: Vec<(Vec<u8>, crypto::sign::PublicKey)>
```

# Drawbacks

This is an addition to a fundamental type and as such must come under scrutiny. There is a feeling 
that this should maybe require a separate type, however this would mean there is existing features
not required by this addition.

# Alternatives

Analysis shows this is not the case, even append type data may be 
transferred and will still require cryptographic security. It can be argued the fundamental type 
may be complicated by this addition. Work was done in looking at potentially creating a state 
type approach or even an identifier approach, but these caused further complexity. Therefore this
is so far considered the most efficient mechanism to include append type data. 

# Unresolved questions

What parts of the design are still to be done?

#Implementation overview

The current structured data type is as follows.
```rust
struct StructuredData {
    tag_type           : u64,
    identifier         : NameType,                     // struct NameType([u8; 64]);
    version            : u64,                          // mutable - incrementing (deterministic) version number
    data               : Vec<u8>,                      // mutable - in many cases this is encrypted
    owner_keys         : vec<crypto::sign::PublicKey>, // mutable - n * 32 Bytes (where n is number of owners)
    previous_owner_keys: vec<crypto::sign::PublicKey,  // mutable - m * 32 Bytes (where n is number of owners)
    signatures         : Vec<crypto::sign::Signature>, // mutable - detached signature of all the `mutable` fields (barring itself) by creating/updating owners
}
```
This allows a transferable cryptographically secured data element, however this requires two additional fields
to be in line with append-able data. 

 
```rust
struct StructuredData {
    tag_type           : u64,
    identifier         : NameType,                          // struct NameType([u8; 64]);
    version            : u64,                               // mutable - incrementing (deterministic) version number
    data               : Vec<u8>,                           // mutable - in many cases this is encrypted
    append_rules       : AppendRule                         // can this item be appended to
    owner_keys         : vec<crypto::sign::PublicKey>,      // mutable - n * 32 Bytes (where n is number of owners)
    previous_owner_keys: vec<crypto::sign::PublicKey,       // mutable - m * 32 Bytes (where n is number of owners)
    signatures         : Vec<crypto::sign::Signature>,      // mutable - detached signature of all the `mutable` fields (barring itself) by creating/updating owners
    apppend_data: Vec<(Vec<u8>, crypto::sign::PublicKey)>,  // Not signed 
}
```





