# Organization Governance — TandemVector Labs

**Scope:** Shared GitHub portfolio and collaboration standards, not a representation of registered company name, legal ownership, shareholding or authority over externally owned projects.

## Joint stewardship

This organization is managed as a **joint technical collaboration**, not the personal brand of one contributor. Both current owners retain equal GitHub Owner-role standing; this document does not grant one the right to unilaterally reassign the other's organization ownership.

Major decisions (organization handle, ownership, member permissions, private repository access, destructive operations, licensing changes, dependency publication, project ownership transfers and public release promotion) should be recorded with a review by the relevant stakeholders. If a second reviewer has not approved a sensitive change, mark the gate **OPEN** instead of calling it final.

## Source authority and release status

The six original-contributor organization forks listed in [Shared development home](COLLABORATION_HOME.md) are the designated **shared development, PR review and new issue intake locations** for both Owners, as described in [Shared GitHub development home](COLLABORATION_HOME.md). Their personal upstream GitHub repositories remain the Git source-lineage parents and the **existing publication/updater authority** until a separately reviewed release migration is accepted. Third-party fork chains retain their project-specific upstream routing and original authorship. A matching Git commit proves code parity at that commit; it does not prove parity of Issues, pull requests, Releases, Wiki, GitHub Actions, packages, secrets, branch policies or product acceptance.

Never claim stable release status based solely on a successful build, GitHub workflow run or copied tag. Keep each project's raw test evidence, Project Brain, release notes, licenses and version history authoritative for that project.

## Security and accountability

- Preserve visibility boundaries: **private must remain private**; verify actual audience before any transfer or copy.
- Keep two-person owner recovery viable; require owners to enable 2FA before enforcing organization-wide 2FA, to prevent involuntary access lockout.
- Prefer reviewable PRs, appropriately scoped permissions, verified status checks, secret scanning and documented rollback.
- Never silently rewrite provenance, force-push, disable security controls or automatically promote candidate builds.
- Route new issues for the six designated jointly developed first-party projects to their **organization** issue trackers. For all other forks follow project-specific active upstream guidance; do not assume GitHub fork ancestry determines the location of new team work.

## Changes and evidence

Record reason, affected namespace/repositories, old and new values, SHA/commit identifiers, checks, risk, restoration path and unresolved gates. After significant governance changes, update the portable organization Project Brain.

**This file describes intended operating policy. It does not certify that all GitHub UI/branch/security controls are enabled.**
