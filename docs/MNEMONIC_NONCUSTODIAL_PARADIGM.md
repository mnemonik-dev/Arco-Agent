# Mnemonic Non-Custodial Paradigm — where the design lives

> **Pickup hint.** The deep design work for making the Mnemonic backend
> **non-custodial + self-sovereign** (and the payment-surface audit) does **not**
> live in this repo — it lives in the **`mnemonik-dev/monorepo`** repository,
> on the same branch name (`claude/arco-agent-capabilities-5irqsg`):
>
> - **Target paradigm + diagrams** → `work/noncustodial-paradigm/design.md`
> - **Payment-surface audit + findings** → `work/payments-robustness/report.md`

## TL;DR of the paradigm (as it affects Arco)

- **No external wallet / no MetaMask.** The client holds one **Mnemonic identity
  seed** and *derives* an Arc/EVM payment key from it; it signs and broadcasts the
  Arc payment tx itself (viem local account → `eth_sendRawTransaction`). The only
  external step is funding the derived address (faucet/sponsor). See design §12, §19.
- **User-signed authorship.** Memories are COSE_Sign1-signed by the user's key,
  not the operator's — so an Arco deliverable/feedback `bytes32` points to a
  *user-signed* artifact (stronger ERC-8004/8183 provenance). Design §1, §5.
- **Non-custodial payment.** Custodial `mnm_` API keys are removed; pay per-call
  via **x402** (needs an **EVM x402 verifier** — the highest-leverage change for
  Arco) or an on-chain allowance. Design §6, §14.
- **Visibility.** Ship **public** now; Arco's confidential deliverables want
  **shared-to-N** (encrypt to `{client, provider, evaluator}`) as a later track.
  Design §17.

## How Arco plugs in

The Arco↔Mnemonic integration (already built on this branch) is isolated behind
`/api/mnemonic/*` in `server.ts`. Moving from operator-fronted billing to
client-derived x402 is a backend change there, invisible to the UI — see
`docs/INTEGRATION_PLAN.md` in this repo for the integration mechanics, and the
monorepo design doc for the protocol-side target.

_This file is only a pointer; the authoritative, diagrammed spec is in the
monorepo._
