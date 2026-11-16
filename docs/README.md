# Bramble design notes

Potential directions, not decisions. Nothing here is implemented. Files are kept short and are expected to change.

## Framing

A mesh version of unRAID: the same forgiving, appliance-like experience, spread over several mismatched boxes, with an object store at the centre and networked Wasm components as the app platform.

| unRAID | Bramble |
|---|---|
| Mismatched disks, added one at a time | Mismatched nodes, added one at a time |
| Each disk has its own filesystem; a dead disk loses only its own data | No striping across nodes; every node holds whole objects and stays useful alone |
| Shares with include/exclude disks | Buckets with a placement rule ("3 copies, one in the barn") |
| Cache pool plus mover | Write lands locally, replicates in the background |
| Red X, replace the disk, rebuild | Node gone, mesh re-replicates |
| Community Applications templates | Component apps with a config form, no YAML |

## Working assumptions

- **Single owner at first.** Every enrolled node is fully trusted. Rights and multiple users come later.
- **Per-node keys from day one.** Membership is a list of keys, never a shared mesh secret. This keeps node removal and later rights possible without a redesign.
- **Hardware order.** Linux-class boxes first, then browsers and phones as nodes and GPU boxes. Microcontrollers are in scope but not a first target.
- **Components only.** No container compatibility lane (see [apps.md](apps.md)).
- **No Kubernetes API to preserve.** K3s is a test workload, not the control plane.

## Files

| File | Topic |
|---|---|
| [node.md](node.md) | What runs on a node, how nodes and browsers connect |
| [storage.md](storage.md) | Mesh S3 |
| [coordination.md](coordination.md) | Membership, gossip and where consensus is needed |
| [apps.md](apps.md) | App platform, supply chain, distribution |
| [simulation.md](simulation.md) | Deterministic testing at two levels |
| [roadmap.md](roadmap.md) | Stages, experiments, open questions |

## Related work

Standalone libraries, already open source, that cover pieces of this design. None of them is Bramble.

| Library | What it is | Where it shows up |
|---|---|---|
| [wasmnet](https://github.com/backkem/wasmnet) | Deterministic virtual IP network made of Wasm components, on virtual time | One virtual network per app ([node.md](node.md)); system-level simulator ([simulation.md](simulation.md)) |
| [wasmshim](https://github.com/backkem/wasmshim) | Linux userspace personality on an emulated AArch64 CPU, as a Wasm component | Real Linux software inside the simulator ([simulation.md](simulation.md)) |
| [codelib](https://github.com/backkem/codelib) | Sandboxed JavaScript on Wasm, with allowlisted egress and host-held credentials | Scripted apps as a pinned engine plus readable source ([apps.md](apps.md)) |
