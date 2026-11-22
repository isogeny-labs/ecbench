# ecbench

The `ecbench` command is copied from [`aburan28/crypto`](https://github.com/aburan28/crypto). Its CLI source lives in `src/bin/`; the benchmark methods, workloads, records, and checkers are imported from the pinned `crypto` library. Changes to those shared APIs belong in `crypto` so there is one implementation.

## Build

```sh
./tools/bootstrap_crypto.sh
cargo build --locked --release
cargo run --locked --release -- methods
```

CI also builds the release CLI, checks that `src/bin/` matches the pinned `crypto` source, runs the smoke spec, and requires `ecbench verify --replay-all` to pass.

The bootstrap fetches a shallow, sparse checkout at the SHA in the script into ignored `deps/crypto`. Besides the fixed patterns, `tools/crypto_inputs.py` reads the pinned source for the path crates Cargo resolves and the files the library embeds with `include_str!`/`include_bytes!`, so a bump needs no hand-kept list. The `crypto pin bump` workflow (`.github/workflows/pin-bump.yml`) does the bump every Monday, and on demand: it moves the pin to crypto's `main`, re-copies the CLI, runs the CI checks, and opens or updates a pull request with their result. CI warns when the pin is more than 14 days old. Review that SHA when updating the CLI, and update the copy and pin together. The source reference is [`crypto`'s ecbench documentation](https://github.com/aburan28/crypto/tree/main/docs/ecbench).

The CLI accepts the same specs and session directories as the command in `crypto`. Generated sessions and comparison outputs are evidence; keep their input, receipt, and replay requirements from the source documentation.

## Golden small-field research suite

[research/golden-smallfields](research/golden-smallfields/README.md) supplies a
Python reference corpus with 56 explicit curves over seven fields, 6,931 arithmetic
known answers, and 69 polynomial relation-search fixtures. It includes isogeny,
Frobenius, GLV2, and GLV/GLS4 evidence, exact relation truth, solver measurements,
and a proof-carrying GF(2) preprocessing experiment.

Run the independent reference checks from that directory:

```sh
cd research/golden-smallfields
python -m pip install '.[algebra,validation]'
python -m unittest discover -s tests -v
python -m ecbench battery-verify
python tools/verify_followup.py
```

See the [research follow-up](research/golden-smallfields/FOLLOWUP.md) for the
measured preprocessing results and remaining leads, or the
[28-page PDF](research/golden-smallfields/docs/ecbench_updated_report.pdf).
The Python cases are reference fixtures and experimental solver measurements;
native Rust workloads and shared record/checker changes continue to belong in
the pinned `crypto` library. Its session receipts and replay requirements apply
when integrating these fixtures into native benchmark runs.

## Licence

MIT, see [LICENSE](LICENSE); to report a vulnerability, and for the acceptable-use policy, see [SECURITY.md](SECURITY.md). A build also compiles the pinned `crypto` source, which carries its own licence terms.
