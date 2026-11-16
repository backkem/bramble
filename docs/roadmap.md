# Stages and experiments

Potential direction. See [README.md](README.md) for framing. The first three stages need no consensus.

## Stages

1. **Two daemons find each other.** Claim, join, gossip. Done when killing one shows up on the other, and the same scenario passes in the deterministic simulator.
2. **Dashboard.** Nodes, disks, capacity, health, "unclaimed node found". Any node renders the whole mesh from gossip. Done when a second node can be claimed from a browser.
3. **Buckets and Files.** Done when pulling the plug on a node loses nothing.
4. **Apps.** Component host, manifests, per-node reconciler, hostile-app test. Done when an app moves to another node on its own.
5. **Secure ingress and remote access.** PWA, passkey as owner credential, signalling server.
6. **Bootable image, phone as witness, GPU apps.**

The `cas` primitive lands between stages 4 and 5, when the first at-most-one app needs it.

## Experiments

| Experiment | Question |
|---|---|
| Mesh-in-a-box simulator | Can every later experiment be tested deterministically? |
| Membership without a master | What does a partitioned node accept after an unseen revocation? |
| Garage under mesh conditions | Adopt, fork or build the store? |
| Control plane in the bucket | Do nodes converge when both halves of a split change desired state? |
| `cas` backends compared | Raft vs CASPaxos-style vs BFT baseline at home scale |
| K3s without quorum | What breaks, and which workload guarantee classes fall out? |
| Component app on the store | Can a stateless component plus a bucket move between nodes with a browser connected? |
| Hostile-app test | Does the platform contain malicious components? |
| Browser tab as a node | Is ingress simply "the browser joins the overlay"? |
| Multi-hop overlay | Identity-addressed streams or an IP overlay? |
| Capability-aware placement | Does one portable binary hold up for accelerators? |
| zkVM compilation proof | Proving cost per kilobyte of source for a small compiler |

## Open questions

- Offline first contact from a stock browser.
- Revocation under partition.
- State for apps that are not "component plus bucket".
- Which first-party apps make the platform useful without a catalogue.
- One implementation language, or a defined seam between Go and Rust.
