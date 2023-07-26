# Hashing

- Hashing is a technique of mapping some data object to some other data object 
  such that it fosters almost constant time lookups and retrievals. 
- As a data structure, it stores data in the form of key-value pairs, where
  keys are required to be unique (hash-set and hash-table).

## Implementation

- A hash table is implemented internally using arrays.
- A hash function is used to compute a unique index for each unique key.
- An important requirement for a hash function to be suitable is that it must 
  not lead to collisions, i.e., no two keys should map to the same location.
- Hashing functions often use modular arithmetic to compute the location from 
  a given key. This causes collisions which are then resolved.

## Load factor

`load factor = n / m`, where  
- `n` is the number of entries occupied in the hash table
- `m` is the number of buckets

Optimal values of load factor range between 0.6 and 0.75.

## Collision resolution techniques

1. **Separate chaining**: When a key generates a location or index which is 
   already occupied, the cell is converted to a linked list and the new value 
   is added a node to this list. The linked list requires a linear search, thus 
   increasing the data retrieval time. The performance can deteriorate very 
   quickly if there are a huge number of keys mapped to the same location.  

2. **Open addressing**: When a key-value has to be inserted, the buckets are 
   examined, starting with the slot returned by the hash computation and 
   proceeding in some probe sequence, until an unoccupied slot is found. When 
   searching for an entry, the buckets are scanned in the same sequence, until 
   either the target record is found, or an unused array slot is found, which 
   indicates an unsuccessful search. Following are some probe sequences:

   - Linear probing: interval between probes is fixed (usually 1).
   - Quadratic probing: interval between probes is increased by adding the 
     successive outputs of a quadratic polynomial to the value given by the 
     original hash computation.
   - Double hashing: interval between probes is computed by a secondary hash 
     function.

