# Membership and coordination

Potential direction. See [README.md](README.md) for framing.

## Idea

Shrink the part that needs consensus instead of picking a different consensus protocol. Joining the mesh should never be a visible cluster operation.

| Tier | State | Mechanism |
|---|---|---|
| 1 | Membership, health, desired state, most data | Gossip plus CRDTs. No consensus. |
| 2 | Conditional writes, at-most-one leases | Single-key consensus over that key's replica set (CASPaxos-style). No leader, no log. Maps onto `If-Match`. |

One interface for tier 2: `cas(key, expected, new)`.

Two-node meshes: a phone or browser tab as a vote-only witness.

## Why not a replicated log

**Raft:**

- Membership is consensus state. Adding a node needs the existing majority.
- Two nodes tolerate zero failures. Home meshes are often one to three nodes.
- Losing a majority needs manual recovery.
- Everything is ordered through one leader, and most state needs no ordering.

**Avalanche (Snow family):** built for thousands of staked validators. Sampling degenerates to polling everyone at home scale, and it tolerates fewer offline nodes than Raft.

**BFT (for example Commonware simplex):** needs `2f+1` of `3f+1`, so four nodes to survive one failure, against faults a single owner does not have. Membership is still a fixed set changed through the log.

"Permissionless" in those systems rests on stake. A home mesh has no stake, so the member set is known once auth is a separate layer.

## Membership

- Signed add and remove grants in a CRDT.
- Open: what a partitioned node accepts after a revocation it has not seen.
- Open: whether pairing with one node can vouch for the whole mesh.
- Changing a key's replica set is still a reconfiguration, only smaller and lazier.

## Workload guarantees

One guarantee does not fit all apps. Candidate classes:

- May duplicate under partition.
- At most one; freezes on the minority side.
- Pinned to a device.

## Where BFT could fit later

- Federation between households that do not trust each other.
- Independent rebuild attestation (see [apps.md](apps.md)).
