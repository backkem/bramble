# Node and connectivity

Potential direction. See [README.md](README.md) for framing.

## What runs on a node

One static binary on any existing Linux. A bootable image comes later. Rust is the likely language, since Wasmtime, smoltcp and Commonware are Rust. go-lp2p stays the spec playground.

| Layer | Job | Possible source |
|---|---|---|
| Identity | Per-node key generated at first boot; signed member list | New; LP2P pairing as the ceremony |
| Links | mDNS discovery, mutually authenticated QUIC | Open Screen Protocol (openscreen-rs as a starting point) |
| Gossip | Membership, health, desired state | New, written against simulation-friendly runtime traits |
| Store | Independent disks, replicated objects, S3 API | See [storage.md](storage.md) |
| Host | Wasmtime, one virtual network per app | [wasmnet](https://github.com/backkem/wasmnet) |
| Reconciler | Runs whatever desired state assigns to this node | New |
| Gateway | Serves the dashboard, routes HTTP to apps | New |

## Node to node

- **Same LAN:** mDNS finds peers, QUIC connects them, the member list says which keys to accept.
- **First node:** advertises as unclaimed; the first browser to reach it claims it.
- **Later nodes:** "add a node" in the dashboard produces an install one-liner or image carrying the mesh public key and a one-time join token. The box boots and joins without a ceremony.
- **Across sites:** relay through any node with two interfaces first. NAT traversal through a signalling server later.

## Browser to mesh

A ladder, because LP2P and Local HTTPS are not in browsers yet.

1. **Plain `http://bramble.local`.** Served by whichever node answers. Enough for a dashboard. Not a secure context: no passkeys, no service worker.
2. **PWA on a real HTTPS origin.** Connects to the local node over WebTransport with a pinned certificate hash. Gives passkeys and offline use after the first load. The PWA must be loaded once from the internet. Local network permission prompts and Safari support need testing.
3. **LP2P and Local HTTPS.** Free once browsers ship them, since the node already speaks OSP.

## Later

- **Browser tab or phone as a node:** component host and network stack in a tab, a data channel or WebTransport as the link, a service worker mapping fetches onto mesh sockets. Prior art: [wasmnet](https://github.com/backkem/wasmnet) already runs its network in a tab, and [codelib-web](https://github.com/backkem/codelib/tree/main/web) hosts components in a Worker.
- **Phone as witness:** a vote-only member for two-node meshes (see [coordination.md](coordination.md)).
- **Multi-hop overlay:** open whether identity-addressed streams are enough or an IP overlay with a routing protocol is needed.
- **Passkey as the owner credential.**
