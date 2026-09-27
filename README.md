# Verifelse Methodology

The public, reproducible scoring methodology behind **Verifelse's VIF score** — a
0–100 rating of how *verifiable* a centralized crypto exchange's Proof-of-Reserves
(PoR) and transparency claims are.

This repository is the open specification of the rubric. It exists so anyone can
read exactly how the score is computed, reproduce it from sourced facts, and
propose corrections. Verifelse is an independent transparency project; scores are
not influenced by any relationship with the exchanges evaluated.

> The VIF score measures **asset-side coverage and the verifiability of
> transparency claims**. It is **not** proof of liabilities or solvency.

## How the score works

The VIF score is a deterministic weighted sum of six components. Each component is
scored 0–100 and clamped, then combined with the weights below (which sum to 1.0).
Inputs are sourced Proof-of-Reserves facts plus an exchange's incident history.

| # | Component | Weight | Measures |
|---|-----------|:------:|----------|
| 1 | Asset Verification | 25% | Scope and assurance of the reserve-asset evidence |
| 2 | Liability Verification | 25% | Scope and assurance of the customer-liability evidence |
| 3 | Proof Freshness | 15% | Publication cadence and absolute age of the latest proof |
| 4 | Public Verifiability | 15% | Whether outsiders can independently confirm the claims |
| 5 | Methodology Transparency | 10% | How well the method and sources are documented |
| 6 | Incident Disclosure | 10% | History of incidents, aged and weighted |

The score is **compute-and-store**: the policy below is applied over sourced facts,
and the resulting per-component sub-scores and rationale are recorded alongside the
data. Values stay static, reviewable, and overridable.

## Sourced input fields

Each exchange's Proof-of-Reserves facts include the following, which drive the
component rubrics:

| Field | Values | Meaning |
|-------|--------|---------|
| `walletDisclosure` | full / partial / none | Published reserve wallet addresses; `full` = signed proof of control |
| `liabilityVerificationMethod` | lpor / zk-global / merkle-sum / merkle-inclusion / auditor / none | Distinguishes a publicly derivable aggregate, a global proof over hidden liabilities, sum commitments, inclusion-only proofs, and private review |
| `thirdPartyAssurance` | type + asset/liability scope + provider | Distinguishes a financial audit, attestation, agreed-upon procedures, or no assurance, and prevents asset-only work from raising the liability score |
| `rootConsistencyCheck` | full / partial / none | Can a user verify their balance under a consistent published Merkle root? |

## Component rubrics

### 1. Asset Verification — 25%
Proving what the exchange holds.

- Audited financial statements — base **95**: customer assets and the broader asset position of the relevant entity are formally audited. Base rises to **100** when the exchange ALSO publishes a Merkle/zk cryptographic Proof of Reserves the public can self-verify (`rootConsistencyCheck != none`).
- Cryptographic asset proof plus asset attestation — base **80**
- Cryptographic asset proof plus agreed-upon procedures — base **70**
- Cryptographic asset proof without independent audit — base **65**
- Standalone third-party asset attestation — base **60**
- Standalone asset agreed-upon procedures — base **45**
- No meaningful asset verification — base **20**
- −15 if the recorded facts are `estimated` rather than `confirmed`

This component measures scope and assurance. Public wallet reproducibility is
scored under Public Verifiability, and the reporting date/cadence under Proof
Freshness. The top tier rewards both formal audit coverage and independent public
verifiability.

### 2. Liability Verification — 25%
Proving what the exchange owes (customer balances).

- Audited financial statements — base **95**: customer obligations and broader liabilities of the relevant entity are formally audited. Base rises to **100** when the exchange ALSO publishes a Merkle/zk cryptographic liability proof (`liabilityVerificationMethod` is lpor / zk-global / merkle-sum / merkle-inclusion).
- Global cryptographic liability proof plus liability attestation — base **70**
- Global cryptographic liability proof plus agreed-upon procedures — base **67**
- Global cryptographic liability proof without independent audit — base **65**
- Standalone third-party liability attestation — base **55**
- Standalone liability agreed-upon procedures — base **45**
- Exchange disclosure without independent verification — base **30**
- base **20** when the program does not prove liabilities
- −15 if the recorded facts are `estimated`

Third-party assurance is scoped independently to assets and liabilities: an
asset-only attestation does not raise this component. Agreed-upon procedures score
below an attestation because the practitioner reports findings without expressing
an assurance conclusion. Differences between LPOR, global ZK, Merkle-sum, and
inclusion-only proofs are scored separately under Public Verifiability.

### 3. Proof Freshness — 15%
Publication frequency and absolute age. The component is the **lower** of the
demonstrated cadence ceiling and the absolute snapshot-age score, so a slow cadence
cannot make an old proof appear as fresh as a frequently updated one.

- Cadence ceilings: continuous **100** · daily **95** · weekly **85** · monthly **75** · quarterly **55** · yearly **35** · ad hoc/unknown **25**
- Absolute age: ≤1 day **100** · ≤7 days **90** · ≤31 days **75** · ≤92 days **55** · ≤366 days **35** · >366 days **15**
- No known latest proof date → **0**

"Continuous" requires continuously updated evidence for *both* the reserve assets
and the liability claim; a live wallet balance without a continuously updated
liability proof does not qualify. Claimed cadence should be supported by publication
history.

### 4. Public Verifiability — 15%
Whether outsiders can independently confirm the claims. Base plus three evidence
factors, clamped 0–100.

- Base **25** (opaque/none → **10**)
- Public liability verification (`liabilityVerificationMethod`): LPOR **+25** · global ZK **+18** · Merkle-sum **+10** · plain Merkle inclusion / auditor / none **+0**
- Merkle root / inclusion check (`rootConsistencyCheck`): full **+25** · partial **+12** · none **+0**
- Wallet disclosure (`walletDisclosure`): full **+25** · partial **+12** · none **+0**

### 5. Methodology Transparency — 10%
How well the method and its sources are documented.

- Base **60**
- **+15** if a canonical proof URL is published
- **+10** if there are at least two independent sources
- **+10** if a cryptographic method is named (Merkle / zk)
- **−10** if the recorded facts are `estimated`

### 6. Incident Disclosure — 10%
History of incidents, aged and weighted. Aging reduces but never fully erases an
incident's historical effect.

- Base **85**, with a component floor of **40**
- Initial penalty by severity: high **15** · medium **8** · low **4**
- Age retention: under 1 year **100%** · 1–3 years **75%** · 3–5 years **50%** · over 5 years **25%**
- Unresolved incidents retain **100%** of the penalty; monitoring incidents retain at least **50%**
- A substantive exchange response reduces the retained penalty by **20%**

## Reproducing a score

The score is deterministic: given the same sourced facts, the rubric always yields
the same result. To reproduce a VIF score, apply each component rubric to the
exchange's sourced facts, clamp each sub-score to 0–100, and take the weighted sum
using the weights in the table above.

## Versioning

This is the **v1** policy. Material changes to weights, rubrics, or input fields are
versioned so past scores remain interpretable. Proposed changes go through the
process in [`CONTRIBUTING.md`](CONTRIBUTING.md).

## License

The methodology in this repository is licensed under
[CC BY 4.0](LICENSE) — you are free to share and adapt it with attribution.
