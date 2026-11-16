# Mesh S3

Potential direction. See [README.md](README.md) for framing.

## Idea

The object store is the centre of the architecture. It holds app data, component binaries and desired state ("these apps are installed, with these settings"). Each node reads desired state and decides locally what to run.

S3 semantics suit a mesh because they are weak: whole-object writes, last writer wins, no renames, no directory atomicity.

## Design rules

- **No striping or erasure coding across nodes.** Each node holds whole objects on independent disks and stays readable alone.
- **Buckets carry a placement rule.** Copy count and location constraints.
- **Writes land locally first** and replicate in the background.
- **Two write tiers:**
  - Plain writes always succeed locally.
  - Conditional writes (`If-Match`, `If-None-Match`) need agreement and fail on the minority side of a partition (see [coordination.md](coordination.md)).

## Prior art: Garage

[Garage](https://garagehq.deuxfleurs.fr/) is an S3-compatible store for mismatched self-hosted nodes. It has no Raft-style consensus: CRDT metadata plus per-object read/write quorums.

Where it does not fit as-is:

- Writes need a quorum of replicas, so an isolated node cannot write by default.
- Membership changes are an operator action (edit and apply a cluster layout).
- It assumes mostly-on servers. A phone or browser tab cannot be a storage member.
- Conditional write support is unconfirmed and should be checked.
- AGPL: fine as a sidecar process, not for linking in.

Possible path: Garage as a sidecar behind Bramble's own store interface, then decide to adopt, fork or build.

## User-facing

- **Buckets** are the share equivalent.
- **Files:** WebDAV and a web file browser over buckets. Desktop file managers mount WebDAV, not S3.

## Open

- Apps that do not fit "stateless component plus synced state", such as SQLite files and large media libraries.
- Placement when a storage member is rarely online.
