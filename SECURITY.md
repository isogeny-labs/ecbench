<!--
Canonical security, disclosure and acceptable-use policy for isogeny-labs
repositories (action plan F4, docs/strategy/2026-11-21-action-plan.md).
Copy it into a repository as SECURITY.md, then fill the two marked sections.
Owner actions before the first copy merges:
  1. enable GitHub private vulnerability reporting on each public repository
     (Settings > Code security > Private vulnerability reporting);
  2. create the security@isogenylabs.io mailbox, or replace the address below.
This is a policy draft, not legal advice. Counsel reviews it with F6 and F9.
-->

# Security policy

## Reporting a vulnerability

Please report security problems privately. Do not open a public issue.

- **GitHub:** use "Report a vulnerability" on this repository's Security tab.
- **Email:** security@isogenylabs.io

Tell us what you found, how to reproduce it, and what you think the impact
is. We confirm receipt within 3 business days and tell you what we will do
within 10 business days. Our default is coordinated disclosure after
90 days, or sooner once a fix is released. We credit reporters who want to be
credited.

We will not pursue legal action against good-faith research that stays
within this policy: it avoids privacy violations, data destruction and
service disruption, uses only accounts and data you own or are authorised
to use, and gives us reasonable time to respond before anything is made
public.

## What this repository is, and what it is not

ecbench is a standalone build of the `ecbench` command from Isogeny Labs'
`crypto` repository: a harness that measures elliptic-curve discrete-log
methods (Pollard rho variants, BSGS, kangaroo, index calculus and others) in
one counted unit and writes sealed, replayable session records. The CLI is in
`src/bin/`; the methods themselves are in the pinned `crypto` source that
`tools/bootstrap_crypto.sh` fetches. It is a research and benchmarking tool,
not production software and not a tool for attacking deployed keys: it solves
only the instances its own workloads generate.

## Acceptable use

Some code here implements published cryptanalytic techniques: attacks on
weak parameters, key recovery from flawed signatures or leakage, and network
probes that read how a system negotiates cryptography. We publish it for
research, education, and defensive assessment.

- Use it only on systems, keys, signatures and data that you own, or that
  you have written authorisation to test.
- Do not use it to obtain anyone else's keys, funds, messages or data, or to
  get into systems you are not authorised to access.
- Network probes complete ordinary handshakes and read what the server
  offers. Do not point them at networks you do not operate or have
  permission to assess, and respect the target's rate limits.
- If you recover key material that is not yours, stop. Do not use it or
  share it; report it privately as above.

## How Isogeny Labs runs client engagements

When we assess a client's systems, these rules hold whatever the contract
says on top of them:

1. **Written authorisation first.** No active test starts before a signed
   authorisation that names the systems, the adversary model, what counts as
   a finding, the testing window and the client's emergency contact.
2. **Public data by default.** We analyse public material (signatures, public
   keys, certificates, handshakes) unless the client agrees in writing to
   provide more.
3. **Recovered key material.** If an analysis recovers a private key or
   other secret, we tell the client's named contact at once through the
   agreed secure channel. We never store it in plaintext, never put it in a
   repository, ticket, report or log, and delete it at the end of the
   engagement, confirming the deletion in writing.
4. **Evidence, not exploitation.** We show a finding with the smallest proof
   that establishes it: a verified result, a test, a signed evidence record.
   We do not use it beyond that.
5. **Third parties.** A weakness found in someone else's product is reported
   to that vendor under coordinated disclosure (90 days by default), with
   the client's agreement where the client's data was involved.

## Ethics

We build attacks so that their cost is measured rather than guessed, and so
that weak deployments are found by their owners first. We do not sell attack
capability, target individuals, or take work whose purpose is to defeat
security that people rely on without their owner's consent.

## Components with dual-use capability

| Path | What it does | Intended use | Safeguards |
|:--|:--|:--|:--|
| `src/bin/ecbench.rs`: `run`, `verify`, `compare` (methods from the pinned `crypto` library) | Solves ECDLP instances with each method and records the work | Measuring and comparing what each method costs | Targets come from the workload: a planted `Q = [k]G` with a scalar derived from the spec, or a public target hashed to the subgroup. Solvers never receive the planted scalar; the runner checks `[k]G = Q` itself |
| `ecbench gpu-stage`, `gpu-match` | Runs `crypto`'s CUDA rho and BSGS benchmark binaries on a frozen workload | GPU cost measurement | Frozen benchmark workloads only; the scalar is checked on the CPU |
| Cheon-family methods (in `crypto`) | Discrete logarithm with a static Diffie–Hellman oracle | Pricing an oracle-assisted attack beside plain ECDLP | The oracle is simulated inside the run from the workload's planted scalar; it never queries a real service |
