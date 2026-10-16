# R039 — Current two-direction fork promotion (2026-10-15)

**Proposed / REVIEW REQUIRED — not effective until merged by authorized human Owners.** This section closes the missing *joint-organization development → original personal release repository* direction. All historical R006 snapshots below remain evidence, not perpetual SHA or product acceptance. No live repository, release, tag, CI secret, or private data was changed by this proposal.

## Scope and invariants

- Applies only to six first-party public GitHub fork pairs: `OpenInternetGateway`, `ChatGPTRemoteCommander`, `DirectInternetMethod`, `FreeNetHub`, `TaskbarMonitorEnhanced` and `PersianWordAgent`; organization owner `Usefull-Skills`, personal parent `GOD13emad`. The two legacy third-party fork chains and personal private repositories are **excluded**. Check exact `fork=true`, `parent.full_name`, numeric repository IDs and visibility before every transfer.
- Organization forks are the **proposed** joint PR/new issue home in [governance PR #3](https://github.com/Usefull-Skills/.github/pull/3); personal parents remain existing published-release/updater authorities. Treat those as different roles until a separately reviewed migration occurs.
- Keep both active Owners' administration, protections, original branches/commits, private visibility, signatures and review rights. No force push, reset, squash/cherry-pick into a source whose ancestry must remain reconcilable, auto-release, or substitution of automated technical review for a genuine independent Owner approval.

## A. Organization-developed code promoted to the personal release source

1. **Snapshot live refs:** independently fetch personal `main` SHA `S0`, organization `main` SHA `O0`, merge-base/ancestry, open PRs and checks. Verify this is the intended fork network and no unreviewed divergence already exists. Do not use historic snapshots as a write lease.
2. **Approve organization development first:** land changes by reviewable PR into organization `main` only after exact-head CI/CodeQL/appropriate regression and a separate, eligible *human* Owner review. Let the resulting accepted organization head be `O1`. `O1` is source-code acceptance, **not** personal stable release acceptance.
3. **Promote via a cross-fork upstream PR:** base `GOD13emad/<repo>:main`, head the accepted `Usefull-Skills/<repo>:main` at `O1` (or a pinned source-equivalent immutable branch). Re-read personal head `S1`, perform source-specific tests, integration checks, and independent review. A normal **merge commit** in the personal parent preserves the exact `O1` ancestor while accepting any concurrent parent-only commits. A true fast-forward may also preserve ancestry where allowed. If source policy enforces linear history, **STOP** for a separately approved reconciliation plan; never assume a squash/rebase can preserve the original `O1` commit identity.
4. **Enforce the ancestry gate before returning to the organization:** after upstream acceptance, let personal protected `main` be `S2`. Verify `O1` is still the organization branch head, `O1` is an ancestor of `S2` (`git merge-base --is-ancestor O1 S2`), and exact-source-HEAD checks passed. If any condition fails, leave refs untouched and reconcile another reviewed PR. Equivalent file content without this ancestry condition does **not** justify blind fast-forward.
5. **Safely reconcile, but only through permitted branch controls:** after all A.4 invariants, attempt the **single-ref GraphQL `updateRefs` with `beforeOid=O1`** as described in the *Verified expected-old ref protocol* below **only if repository rules authorize that exact write to protected `main`**. Use `afterOid=S2` and `force=false`. GitHub REST's `PATCH git/refs` with `force=false` is **not** an exact-old-SHA compare-and-swap; never use GET + PATCH to pretend it is. Any branch protection denial or ref drift is a hard STOP. If direct update is prohibited, use the organization's independently approved PR/merge-queue flow; do not assert resulting main's SHA equals `S2` unless it actually does (merge commits may produce another SHA). Independently re-read original and target refs, branch protections and exact-HEAD checks; preserve other branches/service data and never publish an asset or tag from source-code reconciliation alone.
6. **Release independently:** only the personal-parent publishing/updater process can release until release URLs and product-specific acceptance have separately passed both-Owner review. Validate installer provenance, signed assets if required, real deployment/rollback, security findings and immutable version identity as applicable.

## B. Personal-source changes with no unpromoted organization changes

- Verify current parent/fork refs are the correct intended pair. If org head is an ancestor of parent head, parent advanced cleanly and the fork has no new unreviewed commits, apply only the **approved single-ref `updateRefs` expected-old-`beforeOid` operation** described below (`force=false`), and only if organization branch protections permit it; otherwise take the genuinely reviewed organization PR/merge-queue path. Re-check source/target SHAs and exact-head CI; a merge commit may establish ancestry/content acceptance without establishing **exact-tip SHA parity**.
- If org is ahead, both branches advanced, protection changed, the source PR is not approved, or ancestry is ambiguous, **do not synchronize or reset**. Instead resolve the org changes with path A and a human-reviewed Git merge/PR. Never use `gh repo sync --force` to make the numbers match.

## Verified expected-old ref protocol — GitHub GraphQL, not REST PATCH

**Preflight is mandatory on every run.** Confirm the correct GitHub organization repository's immutable `node_id`, fork parent and numeric repository IDs, visibility, live protected branch rules, actual Owner/reviewer authorization and the accepted source and organization SHA chain. Confirm old organization head is exactly `O1`, new source head is exactly `S2`, and `git merge-base --is-ancestor O1 S2` succeeds with fresh refs. Never use an old checkpoint as authorization to write.

GitHub GraphQL `updateRefs` accepts `UpdateRefsInput(repositoryId, refUpdates)`. The `RefUpdate` input has `name`, `afterOid`, **`beforeOid`**, and `force`. The schema describes `beforeOid` as **“The value this ref needs to point to before the update.”** For **one specific ref**, the intended conditional mutation is:

```graphql
mutation GuardedOrgRefUpdate($input: UpdateRefsInput!) {
  updateRefs(input: $input) {
    clientMutationId
  }
}
```

The example variables below use symbolic placeholders, **not real reusable permission or merge tokens**:

```json
{
  "input": {
    "repositoryId": "<freshly verified organization node_id>",
    "refUpdates": [
      {
        "name": "refs/heads/main",
        "beforeOid": "<current, independently re-read O1 SHA>",
        "afterOid": "<accepted S2 SHA>",
        "force": false
      }
    ]
  }
}
```

Never omit `beforeOid`, never set `force=true`, and do not attempt this direct mutation if branch rules require a reviewed PR or otherwise prohibit the ref update. A request with an expected old SHA is **not a branch-protection bypass**. If rejected for mismatch, permission, required checks, or rulesets, stop and record the exact failure. Re-run ancestry/CI/independent-review preflight before a *separately authorized* retry; do not auto-retry with the freshly fetched head.

**Live behavior verified 2026-10-15:** on a freshly created, **unprotected temporary** branch of `Usefull-Skills/.github`, a GraphQL `updateRefs` request with deliberately incorrect `beforeOid` was rejected and left the ref unchanged; a request with the correct `beforeOid` and `force=false` succeeded and exact SHA readback matched. The temporary branch was deleted and checked absent; protected `main` was checked unchanged. **No direct protected-main write was tested or authorized by this fixture.**

**Protected-main fallback:** open an ordinary cross-fork/organization integration PR with appropriately verified source commits, exact-HEAD checks and actual independent human Owner approval, then use only GitHub's policy-compliant merge/merge-queue flow. After merge, record the **observed** organization tip `O2`. When `O2 != S2` (e.g. a merge commit), classify **EXACT-TIP-PARITY=OPEN** even if `S2` is an ancestor of `O2`; do not claim a clean fast-forward back to the personal source. Assess any new organization commits/differences in a separately reviewed reconciliation before the next synchronization. If neither a permitted conditional ref update nor a genuinely approved PR merge can preserve required provenance, leave branches unchanged and declare the synchronization gate **BLOCKED**, not PASS.

## Reproducible algorithm evidence and limitations

- A separate throwaway local Git fixture (2026-10-15) simulated `O1` (organization work), concurrent `S1` (personal work), an ancestry-preserving merge to `S2` and a clean org `--ff-only` update to the same `S2` SHA: **PASS**. A copy-equivalent personal squash branch deliberately failed `merge-base --is-ancestor O1` as expected. No user source repository was modified by that fixture.
- On 2026-10-15 the six paired org forks had `has_issues=true`, verified parents and matching protected default-head SHAs at the cited checkpoint. Parity is a snapshot, not continuous background mirroring. Human reviews, source release gates and current poststate remain OPEN unless independently evidenced.
- Primary docs: [fork-origin PR](https://docs.github.com/en/pull-requests/how-tos/create-pull-requests/creating-a-pull-request-from-a-fork); [merge commit vs squash/rebase](https://docs.github.com/en/pull-requests/reference/pull-request-merges); [sync a fork](https://docs.github.com/en/pull-requests/how-tos/work-with-forks/syncing-a-fork).

---

## Historical source-to-organization snapshots (R006 and earlier; not current release approval)
# R006.2 — Terminal exact-head verification (2026-10-14)

**Current accepted public-fork Git/CI milestone: PASS.** All **eight** publicly forked repository default branch SHA values match personal upstream (direct GitHub ref readback at this snapshot), and all existing Git Tag name/target SHA entries match in paginated checks; intentional historical fork-only branches are **preserved**, not deleted.

- **FreeNetHub:** org and personal `main=e41b3f669a894b1df5f0e84884eeb8600e747224`; org [CI 38490448851](https://github.com/Usefull-Skills/FreeNetHub/actions/runs/38490448851) `completed/success`, 10/10 jobs PASS.
- **TaskbarMonitorEnhanced:** org and personal `main=c47ac360d46d1e5b57109598e4ae0251b81b2098`; personal upstream [PR #66](https://github.com/GOD13emad/TaskbarMonitorEnhanced/pull/66) MERGED with one CI-only test file. Source main [Build 38490693011](https://github.com/GOD13emad/TaskbarMonitorEnhanced/actions/runs/38490693011) 1/1 PASS and [Native 38490692925](https://github.com/GOD13emad/TaskbarMonitorEnhanced/actions/runs/38490692925) 3/3 PASS. Organization fork also fast-forwarded safely with zero changed files in ancestry comparison, then its exact-head [Build 38491119700](https://github.com/Usefull-Skills/TaskbarMonitorEnhanced/actions/runs/38491119700) 1/1 PASS and [Native 38491119757](https://github.com/Usefull-Skills/TaskbarMonitorEnhanced/actions/runs/38491119757) 3/3 PASS. The previously in-progress fork-CI state is **SUPERSEDED**.
- **Scope**: These are verified Git ref / hosted CI gates; they do **not** certify physical owner-device deployment, production code-signing, stable Release publication, private repository migration, Projects service-data parity, vulnerability-free applications, or organizational FINAL.
- **Remaining external gates**: both owner accounts ready for org-wide 2FA enforcement, GitHub Owner UI handle rename and downstream URLs/API/packages, paid-plan private rules/secret-scanning options, member privileges settings, authorization for private personal repo transfers, GH Projects scope and unresolved OIG advisories.

---

## Prior R006.1/R006/R005 control snapshots — preserved unchanged below

# R006.1 — Additional accepted CI and source/fork reconciliation (2026-10-14)

**CURRENT public-code acceptance:** Exact HEADs of all **eight** organization public Forks match their corresponding personal-source default-branch commit SHA in direct GitHub ref readback. Their Git Tag name+target-SHA inventories match as checked across pages (104 Remote Commander, 16 DirectInternetMethod, 4 FreeNetHub, 22 OpenInternetGateway, 17 TaskbarMonitorEnhanced, 3 PersianWordAgent, 4 LTEMonitor, and zero WSAGAScript). Some historic Fork-only branches are intentionally preserved and **not** deleted; this does not claim complete equality of every branch, GitHub Issue, Release or package.

- **FreeNetHub source + Fork current SHA:** `e41b3f669a894b1df5f0e84884eeb8600e747224`. Latest org [CI run 38490448851](https://github.com/Usefull-Skills/FreeNetHub/actions/runs/38490448851) ended `completed/success`; **10/10** job records independently returned SUCCESS, including Windows fresh installer, Android emulator, iOS simulator and virtual acceptance. **FNH FF+EXACT-HEAD-CI=PASS**. This supersedes the previously queued/running status in R006.
- **Taskbar R163 test-only change upstream ADMITTED:** personal [PR #66](https://github.com/GOD13emad/TaskbarMonitorEnhanced/pull/66) merged at `c47ac360d46d1e5b57109598e4ae0251b81b2098`. Exactly one script file changed, no runtime or installer. Post-merge personal [Build 38490693011](https://github.com/GOD13emad/TaskbarMonitorEnhanced/actions/runs/38490693011) SUCCESS (1/1 job) and [native A+B 38490692925](https://github.com/GOD13emad/TaskbarMonitorEnhanced/actions/runs/38490692925) SUCCESS (3/3 jobs). The organization Fork then fast-forwarded `main` from `a24d2328...` to `c47ac360...` after exact ID, ancestry (ahead=1, behind=0), 0 changed-file comparison and source-CI preflight; independent GitHub post-readback matched both HEADs. **TASKBAR UPSTREAM ADOPTION=PASS; FORK GIT PARITY=PASS**.
- **Taskbar Fork exact-new-HEAD CI** [Build 38491119700](https://github.com/Usefull-Skills/TaskbarMonitorEnhanced/actions/runs/38491119700) and [native A+B 38491119757](https://github.com/Usefull-Skills/TaskbarMonitorEnhanced/actions/runs/38491119757) were `in_progress` on last check, so **FORK POST-SYNC CI=OPEN** until completed success and job readback. Earlier fork runs for pre-merge `a24d2328...` remain valid for that older exact head, not this merge SHA.
- No tags, releases, original projects, branches or private files were deleted or made public. **Product-final / user-device acceptance remains a separate OPEN gate**, even when CI passes.

**Still OPEN outside safe automation:** both owner accounts ready for organization 2FA enforcement; handle migration `Usefull-Skills` → `TandemVector-Labs` by official GitHub Owner UI and URL/API/Packages verification; Member privileges settings; private source consent and paid-plan branch controls; Projects `read:project` scope; two OIG build-toolchain security advisories without listed fixed versions; owner acceptance. Percentage of whole-portfolio completion UNPROVEN.

---

## Earlier R006/R005 control snapshots — preserved unchanged below

# R006 — Current public-fork reconciliation (2026-10-14)

**New accepted state:** `Usefull-Skills/FreeNetHub:main` and `GOD13emad/FreeNetHub:main` now both point to `e41b3f669a894b1df5f0e84884eeb8600e747224` (exact 40-character GitHub API readback). The earlier R005 `f65da9c8...` head is **SUPERSEDED AS CURRENT**, retained below for lineage. The fork only advanced by two commits; GitHub compare returned `ahead_by=2`, `behind_by=0`, merge base equal to old fork `f65da9c8...`. No force push or branch/tag deletion occurred.

**Mutation preflight and cause:** Repo ID `1418018228`, parent `GOD13emad/FreeNetHub`, exact two HEADs, non-fast-forward guard, source CI [38488479939](https://github.com/GOD13emad/FreeNetHub/actions/runs/38488479939) **10/10 jobs SUCCESS**, and zero in-progress target runs were checked before `gh repo sync Usefull-Skills/FreeNetHub --branch main`. The operation result delivery flagged a shell time boundary despite writing a `...SYNC_PASS` receipt; **independent direct GitHub source and fork HEAD readback** independently confirmed success. Do not rerun based on the transport timeout alone.

**Change surface:** `.github/workflows/ci.yml` (+29), `PUBLIC_MANIFEST.json` (updated file hashes), `tests/test_lifecycle_safety.py` (+23), `windows/installer/FreeNetHub.iss` (+44). The installer change introduces a **fail-closed conflicting-install-root guard** to prevent silent split installation; the existing source CI was accepted before mirror advancement. No release/tag publication is implied.

**Downstream exact-head acceptance:** New organization CI [38490448851](https://github.com/Usefull-Skills/FreeNetHub/actions/runs/38490448851) was `queued` on first post-sync check. **CI gate remains OPEN** until this exact-head run is `completed/success` and all jobs pass. Existing R005 CI [38487571483](https://github.com/Usefull-Skills/FreeNetHub/actions/runs/38487571483) is PASS for **its older exact SHA only**.

**TaskbarMonitorEnhanced:** Fork divergence remains **intentional**: org `main=a24d2328f0b70780bd4ebbe25f29c6d3f44cf4a6`, upstream personal `main=86db0d17c438a255a19dbcfb94c5942922712ecd`; fork exact-head Windows native A/B CI PASS, original product remains separate. Upstream [PR #66](https://github.com/GOD13emad/TaskbarMonitorEnhanced/pull/66) is now `READY FOR REVIEW`, `OPEN`, `merged=false`, one CI test script changed. Upstream PR-hosted Build on exact head [38488705989](https://github.com/GOD13emad/TaskbarMonitorEnhanced/actions/runs/38488705989) PASS. **Do not auto-merge or re-release** without separately accepted upstream promotion gates.

**Whole portfolio FINAL remains OPEN**: external Owner secure-2FA readiness, official organization-handle migration and API/package regression, member privileges, privately scoped repo transfer, GH Projects access, unpatched OIG toolchain advisories, cross-product release/owner acceptance. Public controls must not disclose private repository filenames, content, staging paths or personal account 2FA specifics.

---

## Prior R005/R004/R003 control snapshots — retained as evidence (CURRENT claims superseded above)

## R005.1 — Taskbar upstream review proposal (2026-10-14)

**CURRENT:** Organization Fork R163 main CI remains PASS, and upstream personal [Draft PR #66](https://github.com/GOD13emad/TaskbarMonitorEnhanced/pull/66) was opened **without merging**. Exact PR base `86db0d17c438a255a19dbcfb94c5942922712ecd`, head `Usefull-Skills:main=a24d2328f0b70780bd4ebbe25f29c6d3f44cf4a6`; GitHub returned `draft=true`, `merged=false`, `changed_files=1` (`build/Test-Native-Presentation-R155.ps1`). No executable/installer release was changed. The separate **upstream review/CI/owner acceptance gate** is OPEN; personal main remains the reference for product releases. This is not permission to auto-merge a draft.

# R005 acceptance delta — 2026-10-14

**CURRENT: FreeNetHub Fork sync + exact-head CI = PASS.** The previously OPEN/RUNNING FreeNetHub CI gate documented in the R004 section below has now completed and is **SUPERSEDED FOR CURRENT STATUS**.

- GitHub [run 38487571483](https://github.com/Usefull-Skills/FreeNetHub/actions/runs/38487571483) finished `status=completed, conclusion=success` at exact organization-fork `main=f65da9c8ff2ca9e6207aecc3c7e67b6b935ee6eb`. Independent GitHub Jobs readback: **10/10 SUCCESS** (`ios-static`, `linux-static`, `android-build`, `windows-installer-fresh`, `console-virtual-e2e`, `windows-unit`, `windows-virtual-signing`, `ios-simulator-runtime`, `android-emulator-runtime`, `virtual-acceptance`); none failed.
- Main source and organization-fork ref reads match at full SHA `f65da9c8ff2ca9e6207aecc3c7e67b6b935ee6eb`. The change was fast-forward-only and removed a legacy automatic Linux release-publisher job; no forced push, branch/tag deletion or release publication occurred.
- **Scope of PASS:** Git mirror target main-head parity and exact-head hosted GitHub Actions regression; **not** evidence of production installation/owner acceptance, a signed public stable release, or all historical branch/tag service-data parity.
- **TaskbarMonitorEnhanced:** Fork-only PR1 main R163 and exact-head native A+B CI remain ACCEPTED/PASS (see prior R004 delta); personal upstream deliberately remains separate.
- **Remaining critical gates:** two-owner secure 2FA and org enforcement; official handle rename/URL/API/package regression; member privileges/review requirement; consented private-repository scope; GitHub Projects read scope; two unpatched OIG dev-chain GHSA risks and release/owner validation. **Portfolio FINAL remains OPEN.**

---

## R004 previous acceptance snapshot — superseded only where explicitly noted

# Current reconciliation — R004 (2026-10-14)

**CURRENT / CONFIRMED:** Public-fork default branches have **7/8 exact upstream SHA parity** after reconciliation. The one exception is **intentional and tested fork-specific divergence** in TaskbarMonitorEnhanced, not an unsynchronized stale fork. Historic R003 snapshot below is preserved as provenance and is **SUPERSEDED FOR CURRENT STATUS**.

- **FreeNetHub main Fast-forward: PASS.** Upstream and organization fork both point to `f65da9c8ff2ca9e6207aecc3c7e67b6b935ee6eb`. Exact prestate source `f65da9c8...`, fork `cab6139d...`, compare `ahead_by=2, behind_by=0`; `gh repo sync --branch main` succeeded without force; independent GitHub ref readback matched. The new `PUBLIC_MANIFEST.json` matches the updated CI file. The two commits removed a **legacy automatic Linux release-publication job** rather than adding automatic release behavior. No fork-only historical refs were deleted.
- **FreeNetHub exact-head CI: OPEN/RUNNING** in [workflow run 38487571483](https://github.com/Usefull-Skills/FreeNetHub/actions/runs/38487571483). Initial job evidence: iOS static, Linux static, Android build, Windows fresh installer, console virtual E2E, Windows unit, and Windows virtual signing all SUCCESS; mobile iOS simulator and Android emulator acceptance jobs were still running at the last check. Do not call the whole CI PASS until the run completes with SUCCESS.
- **TaskbarMonitorEnhanced fork-specific main acceptance: PASS.** Organization fork `main=a24d2328f0b70780bd4ebbe25f29c6d3f44cf4a6` is 3 commits ahead of personal upstream `main=86db0d17c438a255a19dbcfb94c5942922712ecd` by **intentional R163 diagnostic/paint-readiness changes**. Fork PR #1 merged; [main-head build 38485471888](https://github.com/Usefull-Skills/TaskbarMonitorEnhanced/actions/runs/38485471888) and [native A+B acceptance 38485471910](https://github.com/Usefull-Skills/TaskbarMonitorEnhanced/actions/runs/38485471910) both SUCCESS. [Upstream CI-specific Issue #65](https://github.com/GOD13emad/TaskbarMonitorEnhanced/issues/65) was closed with completion evidence. Preserve the original upstream release and the fork's validated changes; **DO NOT RESET/SYNC AWAY** this fork-specific delta without upstream promotion authority.
- All other first-party and legacy public fork default branches were SHA-matched in the R004 readback. A default-head check is not exhaustive branch/tag parity, so continue per-ref verification when required.
- Organization URL remains `Usefull-Skills`; displayed brand `TandemVector Labs`. Shared ownership, 2FA enforcement, private replication and rename acceptance are separate critical gates, not closed by code parity.

**Accepted gate outcomes:** `FNH-FAST-FORWARD=PASS`, `TASKBAR-ORG-FORK-MAIN-CI=PASS`; `FNH-EXACT-HEAD-CI=OPEN`, `TASKBAR-PRODUCTION-RELEASE=OPEN`, `WHOLE-ORGANIZATION-FINAL=OPEN`. 

---

## R003 historical snapshot — SUPERSEDED FOR CURRENT STATUS (retained verbatim)

# Public Fork Synchronization Control — TandemVector Labs

**Audit date:** 2026-10-14  
**Status:** PARTIAL / NOT CONTINUOUSLY SYNCHRONIZED  
**Original source owner:** `GOD13emad`  
**Organization owner/namespace:** `Usefull-Skills` (displayed brand: TandemVector Labs)

This document records GitHub **read-only evidence and a safe synchronization rule**; it does not itself synchronize repositories, transfer Issues or create releases.

## Verified default-branch snapshot

| Public repository | Authoritative personal head | Organization-fork head | Gate |
| --- | --- | --- | --- |
| ChatGPTRemoteCommander | `6f52f93cbc04ba30354b35cd144ffc96aaa20aba` | same | SHA MATCH |
| DirectInternetMethod | `26ef644117ea474b487186ce5e6e61e92cd02dd4` | same | SHA MATCH |
| FreeNetHub | `f65da9c8ff2ca9e6207aecc3c7e67b6b935ee6eb` | `cab6139d32c149b08ef69bc60a0d58c3c18e6f14` | **OPEN — FORK BEHIND** |
| OpenInternetGateway | `23f4fe514698102c7ecb735c7843fe25c594c8af` | same | SHA MATCH |
| TaskbarMonitorEnhanced | `86db0d17c438a255a19dbcfb94c5942922712ecd` | same | SHA MATCH |
| PersianWordAgent | `65aa3f85a9dccc23fcc19e26e4515ef7b5928697` | same | SHA MATCH |
| LTEMonitor (master) | `646c05592c3497e94ec4aa576aa06c8c3c6f0edf` | same | SHA MATCH |
| WSAGAScript | `99f034ef529aa39aa008e02befe8788cb9d86b46` | same | SHA MATCH |

**Evidence:** GitHub REST `git/ref/heads/{main|master}` read-back on both owners; full 40-character SHA checked in the same audit.

## FreeNetHub — critical sync delta

GitHub compare using base `cab6139d32c149b08ef69bc60a0d58c3c18e6f14` and head `f65da9c8ff2ca9e6207aecc3c7e67b6b935ee6eb` returned **status=ahead, ahead_by=2, behind_by=0**, with two changed files:

- `.github/workflows/ci.yml`: 51 removed lines (must verify CI gate semantics, not blindly assume equivalent coverage).
- `PUBLIC_MANIFEST.json`: checksum/metadata changes (validate exact affected source and manifest consistency before accepting promotion).

**Current source branch count: 2; fork branch count: 10.** The fork has eight additional historical branch refs that no longer exist in the personal repository. These must **not** be auto-deleted just to make counts equal. Classify each as historical retained evidence, superseded work, or unknown before any removal.

**Allowed mutation:** one non-force, fast-forward-only update of `Usefull-Skills/FreeNetHub:main`, guarded by **exact** source/destination head SHA and compare status ahead/behind. STOP if either head changed or downstream branch protection/rules are incompatible. Re-read both 40-character heads and inspect exact-head CI + `PUBLIC_MANIFEST.json` after update. Preserve former fork commit as immutable Git history. Do not modify the personal source.

**Current result:** no synchronization was performed in this audit. `FreeNetHub` parity remains OPEN.

## Rules for all public forks

1. The named personal upstream is the code/release authority unless a documented project decision promotes another source; do not silently switch remotes.
2. Record source/target repository IDs and exact SHA before any branch change; an HTTP success code alone is insufficient.
3. Synchronize only fast-forward-compatible branches. Never force push, blindly recreate unrelated tags, mirror-delete branches, squash history, or overwrite a divergent fork.
4. Recheck complete branch/tag refs with pagination; compare names and SHAs. `main` equality alone is not complete Git ref parity.
5. Preserve Issues, pull requests, Releases, Wiki, packages, CI environment and authentication boundaries: **GitHub Fork is not full GitHub service migration**.
6. Do not label an untested downstream CI/release gate PASS based on upstream tests.
7. Update the cumulative Project Brain and the [central acceptance issue](https://github.com/Usefull-Skills/.github/issues/1) after verified milestones.

## Additional downstream gates

- [TaskbarMonitorEnhanced fork native-GUI failure](https://github.com/GOD13emad/TaskbarMonitorEnhanced/issues/65): fork-hosted Windows lane A still fails physical UI acceptance despite upstream build PASS. Do not release-promote from failed fork tests.
- [OpenInternetGateway npm advisory assessment](https://github.com/GOD13emad/OpenInternetGateway/issues/6): two build-toolchain transitive advisories need independent mitigation and acceptance; no fixed version was listed by GitHub advisories as of this audit.
- GitHub App/CLI authorizations and organization owner 2FA must remain inside approved privilege boundaries.

**Definition of Done:** an independently reproduced, read-back-confirmed ref parity snapshot; verified branch-rule continuity; no secrets/visibility changes; no deleted history; applicable exact-head CI/packaging regression acceptance; cumulative Brain updated. Until these pass, **FINAL=OPEN**.
