# Apps and supply chain

Potential direction. See [README.md](README.md) for framing.

## Idea

App catalogues are a supply chain risk. Wasm components can change that, if the platform keeps authority narrow and distribution content-addressed. A small set of first-party apps is preferred over a large catalogue of ambient-authority containers.

## What components give

| Risk | Containers and plugin systems | Components |
|---|---|---|
| Ambient authority | Container gets the LAN and outbound internet; plugins share one process and its secrets | A component can only call what it imports; imports are readable before it runs |
| Network reach | Whole LAN by default | One virtual network per app; default-deny egress, no LAN |
| Artifact identity | Mutable tags, one image per architecture | One portable binary, one hash on every node |
| Malicious update | Runs with existing privileges | Added imports show up as a diff and can require re-consent |

Home automation example: an integration is a small component granted "talk to this one device", exporting a typed device interface. A rule imports device interfaces and gets no network.

## What they do not fix

- **Abuse inside the grant.** Data access plus internet egress is the dangerous pair. Flag it at install.
- **App web UIs.** App JavaScript runs in the browser, outside the sandbox. One origin per app; the dashboard origin never serves app content.
- **Dependencies inside one component** share its privileges. Isolation exists only at component boundaries.
- **Noisy imports.** Toolchains import broad WASI worlds. Owner grants are the real boundary, not the import list.

## Native code

No container lane. GPU inference, codecs and radios become host-provided interfaces that ship with the node. Apps are components that import them.

## App manifest

Components, buckets needed, routes, required capabilities (GPU, a radio), config form schema. Stored as an object in the store.

## Distribution

- **Install by hash.** Names are local labels. An update is a new hash signed by an already-pinned publisher key.
- **No central catalogue.** An app is an object in a bucket and can be handed device to device.
- **Cooldown plus capability diff.** Hold updates for some days; auto-apply only if imports are unchanged.
- **Independent rebuilds.** A portable artifact lets several parties rebuild and attest to the same hash.
- **Ship source where possible.** For scripted apps: a pinned engine component plus readable source.
- **Compilers as components.** Same output hash in every household, so hashes can be cross-checked.

## Verifiable builds and review (speculative)

A proof is worth having only when the check is expensive to repeat or its input is private. Cheap public checks (imports, grant diffs) are re-run by the node at install.

| Idea | State (late 2026) | Use here |
|---|---|---|
| Reproducible builds with rebuilders | Shipping | Baseline |
| Proof of compilation in a zkVM | [Research prototype](https://arxiv.org/abs/2602.11887) with a small C compiler | Side experiment with a small compiler targeting Wasm |
| Proof of a hosted model's response via TLS | Alpha; third parties must trust a notary | Weak |
| ZKML of an AI reviewer | [Small models only](https://eprint.iacr.org/2026/1112), slow | Not yet |

Caveats:

- A proof shows a review ran, not that it was good. A public fixed reviewer is an oracle an attacker can iterate against.
- Proof of compilation does not cover malicious source.
- More practical: run the reviewer on the mesh's own GPU node with a private prompt; share verdicts as signed statements.
- Zero knowledge fits private inputs, for example a closed-source app proving it was built from source that passed a given check.

## Hostile-app test

- Four malicious components: bucket exfiltration, LAN scan, an update that adds an import, a phishing UI. Check each is contained.
- Port one real device integration and one rule, to see whether narrow grants can express real apps.
