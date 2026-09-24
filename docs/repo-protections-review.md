# Repo protection review — Electric-Boat-Team.github.io

Written 2026-09-24. Two parts: what the team is, and whether this repo's
guardrails are adequate.

## About the team

Terrapin Works is the rapid-prototyping and advanced-manufacturing arm of the
A. James Clark School of Engineering at the University of Maryland. Founded in
2014, it runs 200+ machines across ~28,000 sq ft of lab space
([terrapinworks.umd.edu](https://terrapinworks.umd.edu/)).

The **Terrapin Works Electric Boat Team (TWEBT)** is a new student team spun out
of that program, founded in 2026. It designs, builds, tests, and races electric
marine propulsion systems. The stated goal is to convert an ultra-light
10'6" carbon-composite catamaran into an electric hydrofoil capable of
sustained flight at 20+ knots for the **ASNE Promoting Electric Propulsion
(PEP) East 2027** regatta, while developing modular control suites for
uncrewed surface vehicle (USV) research.

- Website: [electricboat.umd.edu](https://electricboat.umd.edu/)
- LinkedIn: [company/tw-electric-boat](https://www.linkedin.com/company/tw-electric-boat)
- Instagram: [@twelectricboat](https://www.instagram.com/twelectricboat/)
- GitHub org: [Electric-Boat-Team](https://github.com/Electric-Boat-Team)

The PEP competition is run by the American Society of Naval Engineers (ASNE)
with funding from the Office of Naval Research (ONR). PEP 25 drew 36
universities and 45 boats to Virginia Beach
([navalengineers.org](https://www.navalengineers.org/Education/PEP-Workforce-Development/Sponsors)).

Leadership:

- **Tyler Lumpkin** — President and Chief Engineer. Mechanical engineering
  junior; technical supervisor at Terrapin Works.
- **Brian Palmer** — Faculty advisor and founder. Director of Terrapin Works,
  based at the Baltimore makerspace. Competed three times at PEP with
  Washington College, winning the Manned category in 2022 and the Manned
  Displacement category in 2024
  ([faculty.eng.umd.edu](https://faculty.eng.umd.edu/clark/staff/2034/)).

The org currently has six members and five repos. Public: `monorepo`,
`.github`, and this Pages site. Private: `EngbrechtMotorFoil` and
`sponsorship-packet`.

## What this repo protects today

The `.github.io` site is built and published from `main` (Pages source: `main`,
`/`, HTTPS enforced). The `github-pages` environment is locked to the `main`
branch. So **any push to `main` goes live on the public site.**

| Control | Status on this repo |
|---|---|
| Force pushes | Blocked (ruleset `protect main`) |
| Branch deletion | Blocked |
| Linear history | Required |
| Pull request required before merge | **No** |
| Required approving reviews | **No** |
| Required status checks | **No** (no CI exists here) |
| Signed commits | **No** |
| Secret scanning | **Disabled** |
| Push protection | **Disabled** |
| Dependabot security updates | **Disabled** |
| Delete branch on merge | Off |
| Org-wide 2FA requirement | **Off** |

The only protection is the repository ruleset `protect main` (id 23968321),
whose active rules are `deletion`, `non_fast_forward`, `required_linear_history`,
and `creation`. There is no `pull_request` rule, so the branch blocks
history-rewriting but does **not** gate direct commits.

For contrast, the private `monorepo` does have classic branch protection:
1 approving review, stale-review dismissal, re-approval required after new
pushes, admins enforced, linear history, no force pushes or deletions. Its
README's claims match reality. This repo does not.

Six accounts have write access to this repo. All six are org members.

## Is it adequate?

Partly. Force pushes and deletion are blocked, which is the minimum. But for a
repo that auto-publishes to a public website, it is weaker than it should be:

1. **Anyone with write access can push straight to `main` and deploy.** The
   monorepo requires review; this repo does not. The org profile README says
   "everything gets reviewed" — that isn't enforced here.
2. **No CI or status checks.** Nothing validates a change before it goes live.
3. **No secret scanning or push protection**, on a public repo. If a token or
   key ever lands in a commit, it is exposed immediately and unrecoverably.
   Push protection is free for public repos and should be on.
4. **No org-wide 2FA.** An org member's compromised password is enough to push
   to `main`, since the account itself is the only gate.
5. Minor: no `CODEOWNERS`, no community-health files (GitHub scores this repo
   at 12% health), and `delete_branch_on_merge` is off.

## Suggested fixes

- Add a `pull_request` rule to the existing ruleset: require 1 approval (or at
  least require a PR), dismiss stale reviews, require conversation resolution.
- Enable secret scanning and push protection (Settings → Code security). Free
  for public repos.
- Turn on Dependabot alerts and security updates.
- Enforce 2FA across the org (Settings → Authentication security).
- Enable "Automatically delete head branches."
- Add a `CODEOWNERS` file and the missing community-health files.
- If the Pages site ever handles anything beyond static content, gate deploys
  behind the `github-pages` environment with a required reviewer.

None of this replaces the monorepo's protections — it brings this repo up to
the same bar.