# Contributing to the Verifelse Methodology

Transparency improves when the methodology is open to scrutiny. This repository is
the public specification of the VIF score, and we welcome three kinds of
contribution:

1. **Data corrections** — an exchange's sourced facts are wrong or out of date
   (e.g. a proof is more recent than recorded, an auditor changed, a wallet
   disclosure is broader than noted).
2. **Methodology issues** — a rubric produces a result that misrepresents how
   verifiable an exchange's claims are, a component is weighted in a way you can
   argue against, or an edge case is unhandled.
3. **Clarity fixes** — wording, examples, or documentation that make the rubric
   easier to understand or reproduce.

## Ground rules

- **Evidence over assertion.** Every claim about an exchange must cite a primary,
  publicly checkable source (the exchange's PoR page, an auditor's report, an
  on-chain address, a regulator filing). Corrections without a verifiable source
  cannot be accepted — this is the core principle of the project.
- **Independence.** Verifelse does not adjust scores based on relationships with
  the exchanges evaluated. Contributions that amount to reputation management,
  rather than evidence, will be declined.
- **Determinism.** The rubric must produce the same score from the same facts.
  Proposals that introduce subjective or non-reproducible judgment should explain
  how the result stays reproducible.
- **No solvency claims.** The VIF score measures asset-side coverage and the
  verifiability of transparency claims — not liabilities or solvency. Proposals
  should respect that scope.

## How to propose a change

**For a data correction**, open an issue titled `data: <exchange> — <what is wrong>`
and include:
- the field or claim that is incorrect,
- the correct value,
- a link to the primary source that supports it, and
- the date of that source.

**For a methodology change**, open an issue titled `methodology: <component> —
<summary>` and include:
- which component and rule you are addressing,
- what the current rubric produces and why it is wrong or incomplete,
- your proposed change, and
- worked examples showing the before/after effect on at least one exchange.

**For clarity fixes**, a pull request editing `README.md` directly is welcome.

## Review and versioning

- Data corrections are applied once the source is verified.
- Methodology changes are discussed in the issue first; accepted changes bump the
  policy version (currently **v1**) so previously published scores remain
  interpretable. We do not silently change how the score is computed.
- Maintainers may decline changes that cannot be sourced, that break determinism,
  or that fall outside the scope of the methodology.

## Reporting sensitive information

If you believe an exchange is misrepresenting its reserves or you have
evidence that should not be posted publicly, contact the maintainers privately
rather than opening a public issue.
