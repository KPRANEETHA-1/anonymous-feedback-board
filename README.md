# Anonymous Feedback Board

Built on [Midnight Network](https://midnight.network/) for the MLH Midnight Hackathon (Aug 28–30, 2026).

## What it does

A privacy-preserving feedback board: anyone can post a message, and only the original author can remove it — **without the blockchain ever revealing who posted what.**

This is built on Midnight's `example-bboard` template, adapted into a real-world use case: anonymous feedback (for a workplace, community, or classroom) where people need to speak freely, but authorship of a post should never be publicly traceable, while still guaranteeing that only the real author can retract their own words.

## The privacy problem this solves

Most "anonymous" feedback tools aren't actually anonymous — the platform operator can always see who posted what behind the scenes. This project uses **zero-knowledge proofs** to make a stronger guarantee: the smart contract itself enforces "only the author can delete this," without the contract, the network, or anyone else ever learning the author's identity. The proof is generated locally and only proves *that you're allowed to act*, not *who you are*.

## Tech stack

- **Compact** — Midnight's smart contract language (the `post` / `takeDown` circuits)
- **Zero-knowledge proofs**, generated via a local proof server (Docker)
- **React** web UI
- **Lace Wallet** for signing transactions on Midnight Preprod testnet

## What we changed from the original template

- Reframed and relabeled the generic "bulletin board" example into an anonymous feedback board use case
- Adjusted the language-version pragma and upgraded the Compact compiler to `0.31.1` to match the runtime dependencies (`compact-runtime` 0.16.0 / language spec 0.23) pinned in this template's package.json
- Full environment setup from scratch on Windows via WSL2 (Compact's toolchain doesn't support Windows natively)

## How to run it locally

**Prerequisites:** Node.js v24+, Docker Desktop, Lace Wallet (Chrome extension), Compact compiler 0.31.1+

```bash
npm install
cd contract && npm run compact && npm run build && cd ..
cd bboard-ui && npm run build && cd ..
cd bboard-cli && docker compose -f proof-server-local.yml up -d && cd ..
```

In Lace: set Network to **Preprod**, Proof server to **Local (http://localhost:6300)**, and fund your wallet via the Preprod faucet.

```bash
cd bboard-ui && npm run build:start
```

Open `http://127.0.0.1:8080`, connect Lace, and create or join a board.

## A note on where this stands

Midnight's shared Preprod testnet saw heavy load during this hackathon weekend (expected, with hundreds of participants deploying and testing contracts simultaneously). We hit intermittent proof/deployment delays as a result — the full stack (contract compiled, proof server running locally, wallet funded and connected) is verified working end-to-end; final on-chain confirmation was still in progress as of submission due to network conditions outside our control.

## License

Apache-2.0, inherited from the original [midnightntwrk/example-bboard](https://github.com/midnightntwrk/example-bboard) template.
