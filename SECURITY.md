# Security Policy

## Reporting a Vulnerability

If you discover a security vulnerability in **Instella**, please **do not** open a public GitHub issue, pull request, or discussion. Public disclosure of an unpatched vulnerability puts users at risk. Instead, report it privately so the maintainers can investigate, prepare a fix, and coordinate disclosure.

### How to report

- **Preferred:** Use [GitHub Private Vulnerability Reporting](https://github.com/AMD-AGI/Instella/security/advisories/new) on this repository (Security tab → "Report a vulnerability"). This keeps the discussion private to the maintainers.
- **Alternative:** Contact the code owners listed in [`.github/CODEOWNERS`](./.github/CODEOWNERS) directly via internal AMD channels (email or Slack).

Please include:

- A clear description of the vulnerability and its potential impact
- Steps to reproduce (proof-of-concept code, configuration, affected commit/branch/release)
- Affected versions, models, or scripts
- Any suggested mitigations, if known
- Your contact information for follow-up
- Whether you wish to be credited in the published advisory

### What to expect

- **Acknowledgement:** within 5 business days of your report
- **Initial assessment & triage:** within 10 business days
- **Fix timeline:** depends on severity; you will receive periodic updates
- **Coordinated disclosure:** we will work with you on a disclosure timeline; please give us a reasonable opportunity to release a fix before public disclosure
- **Credit:** with your permission, we will credit you in the published advisory once the fix is released

## Supported Versions

Security fixes are applied to the `main` branch and the most recent published release. Older releases and tags are not maintained for security updates — please upgrade to the latest release to receive fixes.

## Scope

In scope:

- Source code in this repository (`instella/`, `hf_instella/`, `scripts/`, `tokenizers/`, `configs/`)
- Build, training, inference, and evaluation scripts
- Configuration templates and example code checked into the repository
- Model loading and tokenizer code that processes untrusted input

Out of scope:

- Vulnerabilities in third-party dependencies (please report upstream — see [`NOTICES`](./NOTICES) for the dependency list). If a dependency vulnerability is exploitable through Instella in a non-obvious way, we still want to hear about it.
- Issues requiring physical access to a machine
- Findings from automated scanners without a demonstrated impact
- Misuse of the model itself for harmful generation (please see the model license for use restrictions)

## Secrets & Sensitive Data

- **Never** commit credentials, API keys, tokens, model weights under NDA, or customer data to this repository.
- Local secrets must live in `.env` files (already excluded by [`.gitignore`](./.gitignore)).
- CI/CD secrets must be stored in **GitHub Actions Secrets**, not in repository files.
- If you accidentally commit a secret: rotate it immediately, then contact the code owners. Removing the file in a follow-up commit is **not** sufficient — the secret remains in git history.
