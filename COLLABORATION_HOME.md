# Shared GitHub development home

**Effective scope (2026-10-15):** TandemVector Labs operates the GitHub organization
[`Usefull-Skills`](https://github.com/Usefull-Skills) with two equal, active **organization Owners**:
[`GOD13emad`](https://github.com/GOD13emad) and
[`mohammadmohammadzadeh19960-code`](https://github.com/mohammadmohammadzadeh19960-code).
Each has repository **Admin** authority throughout the organization's public and private repositories.
This does **not** confer ownership of repositories hosted in someone's personal account.

## Where to collaborate

| Project | Shared team development and review | New issue reports | Existing published releases / updater source |
| --- | --- | --- | --- |
| OpenInternetGateway | [Org code and PRs](https://github.com/Usefull-Skills/OpenInternetGateway) | [Org Issues](https://github.com/Usefull-Skills/OpenInternetGateway/issues) | [Personal original releases](https://github.com/GOD13emad/OpenInternetGateway/releases) |
| ChatGPTRemoteCommander | [Org code and PRs](https://github.com/Usefull-Skills/ChatGPTRemoteCommander) | [Org Issues](https://github.com/Usefull-Skills/ChatGPTRemoteCommander/issues) | [Personal original releases](https://github.com/GOD13emad/ChatGPTRemoteCommander/releases) |
| DirectInternetMethod | [Org code and PRs](https://github.com/Usefull-Skills/DirectInternetMethod) | [Org Issues](https://github.com/Usefull-Skills/DirectInternetMethod/issues) | [Personal original releases](https://github.com/GOD13emad/DirectInternetMethod/releases) |
| FreeNetHub | [Org code and PRs](https://github.com/Usefull-Skills/FreeNetHub) | [Org Issues](https://github.com/Usefull-Skills/FreeNetHub/issues) | [Personal original releases](https://github.com/GOD13emad/FreeNetHub/releases) |
| TaskbarMonitorEnhanced | [Org code and PRs](https://github.com/Usefull-Skills/TaskbarMonitorEnhanced) | [Org Issues](https://github.com/Usefull-Skills/TaskbarMonitorEnhanced/issues) | [Personal original releases](https://github.com/GOD13emad/TaskbarMonitorEnhanced/releases) |
| PersianWordAgent | [Org code and PRs](https://github.com/Usefull-Skills/PersianWordAgent) | [Org Issues](https://github.com/Usefull-Skills/PersianWordAgent/issues) | [Personal original releases](https://github.com/GOD13emad/PersianWordAgent/releases) |

**This table designates the organization repositories as the collaboration/candidate-development home for these six first-party projects, not yet the product release source.** The existing personal repositories remain GitHub *upstream parents* and retain the **existing publication/updater authority** for releases, updater endpoints, and their historic issues/PRs until a separately reviewed release migration succeeds. Do not silently replace the release URL in an installed client.

For the two legacy third-party fork chains (LTEMonitor and WSAGAScript), continue following [the project catalog](PROJECT_CATALOG.md) and that project's own maintainer guidance; a generic policy must not claim transfer of third-party ownership, licensing or release authority. Private repositories are **not cataloged publicly** and must remain private.

## Safe two-owner workflow

1. Propose new collaborative code in the **organization repository**. Use a narrowly scoped branch and PR; keep protected `main` unchanged until checks and an independent authorized review pass.
2. Either organization Owner can administer the organization projects. For sensitive changes, the **other actual human Owner** reviews; do not substitute a second login or automated approval for independent judgment.
3. Validate at the **exact candidate SHA**. A green CodeQL PR scan does not close an alert on protected `main`; prove closure after the accepted merge and a new main scan.
4. Preserve personal-source provenance and existing publication URLs. Fork reconciliation must use explicit SHA/ancestry and approved non-force syncing, **not** overwriting organization-only PRs or release assets.
5. Treat migration of releases, workflows, secrets, existing Issues/PRs, tags, notifications and package registries as separate gated work. Document original and destination identifiers, compatibility, backup/rollback, and reviewer decision. Personal-source repo transfer, deletion, archival, visibility changes and billing changes are **not authorized by this document**.

## Verified boundaries and open decisions

- **Organization collaboration access:** both active Owners and Admin permissions, verified via authenticated GitHub Owner and repository permission readbacks.
- **Org issue intake for all six first-party projects:** `has_issues=true` confirmed via GitHub REST after an authorized, reversible setting change. Historic issues in the personal repos are **not migrated**.
- **Upstream relationship:** all six org repositories remain public GitHub forks of `GOD13emad` originals; their protected default-branch SHA values matched the parent default-branch SHAs at the 2026-10-15 readback. This is **not** a detached independent upstream.
- **Code delivery:** organization PRs for these first-party projects require genuine independent Owner review and accepted tests before any main merge.
- **Release migration:** still **OPEN**. The current applications and release automation reference personal GitHub namespaces; no release endpoint, artifact, tag or GitHub ownership transfer was changed.
- **Policy publication:** edits in this document require accepted organization branch protection and authorized review to become effective on the default branch. Until then, any PR containing this text is a candidate proposal, not an already published governance policy.

For details of repository provenance and per-project support, see [Project catalog](PROJECT_CATALOG.md), [Governance](GOVERNANCE.md), [Fork synchronization](FORK_SYNC_CONTROL.md) and [Contributing](CONTRIBUTING.md).
