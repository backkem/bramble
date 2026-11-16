# Simulation

Potential direction. See [README.md](README.md) for framing.

## Idea

Test the mesh deterministically instead of with boxes on a desk. Two levels:

| Level | Tool | Tests |
|---|---|---|
| Protocol | Deterministic runtime plus simulated p2p | Bramble's own protocols: membership gossip, `cas`, replication. Thousands of seeded runs. |
| System | [wasmnet](https://github.com/backkem/wasmnet): nodes as components on a virtual smoltcp network in one process | Real binaries on a real IP stack: whole-node behaviour, third-party software |

## Protocol level: borrow from Commonware

The [Commonware monorepo](https://github.com/commonwarexyz/monorepo) has the pattern:

- **Runtime seam.** Protocol code is written against traits (`Clock`, `Network`, `Storage`, `Spawner`) with production and deterministic backends.
- **`p2p::simulated`.** Links with configurable latency, drops and bandwidth, changeable mid-run for partitions and nodes that go offline and return. Reproducible from a seed.
- **Storage fault injection.** Partial writes, for power loss mid-write.
- **`glue::simulate`.** A declarative plan, a team that starts, crashes and restarts nodes, and property checks.

The testing infrastructure is the part to adopt or imitate, not the consensus (see [coordination.md](coordination.md)).

## System level: wasmnet as simulator

- Scripted partitions, loss and clock skew on the virtual network.
- Same seed, same bytes: a failing run replays exactly, with a pcap per link.
- Can host real software through a Linux personality. [wasmshim](https://github.com/backkem/wasmshim) is that personality as a standalone component: unmodified AArch64 Linux binaries, no kernel, deterministic.

## Joining the two

A wasmnet-backed implementation of the runtime's `Network` trait would let the same protocol code run in both simulators.

## Scenarios

- Mesh sizes 2, 3, 5 and 9.
- Nodes asleep 30% of the time.
- A split into two halves.
- A node joining during a partition.
- Permanent loss of a majority.
- Both halves changing the app list while split.

Measures: availability of `cas`, time to admit a node, number of recovery steps that need a human.
