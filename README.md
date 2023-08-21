# Allstar configuration for pauldoomgov

![AllStar Run](https://github.com/pauldoomgov/.allstar/actions/workflows/allstar-run.yml/badge.svg)

[Allstar](https://github.com/pauldoomgov/.allstar) is a security-policy GitHub app. It is
installed on this org, and this repo contains the configuration for that app. It
is configured to create issues on repos that do not comply with the configured
policy.

AllStar is set to "complain mode" at this time and can not auto-remediate issues.

## Enabled Repos

AllStar is currently configured to opt-in. If you have a repository in this
organization that you would like added:

* Open a PR that modifies `allstar.yaml` to include your repository
* Once accepted and merged, check the `Issues` tab in your repository for AllStar's findings

## Policy Configuration

These are the expected settings to be in compliance

### [AllStar Main Config](allstar.yaml)

Opt-in repos are listed here. Update this file to monitor more repositories.

### [Action Restrictions](actions.yaml)

No rules currently set.

### [Admin Requirements](admin.yaml)

|   |   |
| - | - |
| Owner-less repositories allowed | false |
| Individual users allowed to be admins | true |
| Maximum individual admins users | 4 |
| Teams allowed to be admins | true |
| Maximum admin teams | 2 |

### [Binary Artifacts](binary_artifacts.yaml)

No binary files are currently ignored. You should override this policy
in your repository and set `ignoreFiles` to a list of the expected in-repo
binaries you wish to allow.

### [Branch Protection](branch_protection.yaml)

Sets baseline controls to ensure the change control process is followed
for code to reach `main`.

| | |
| - | - |
| Branches enforced | default |
| Require approval | yes |
| Approvals required | 1 |
| Dismiss stale reviews | yes |
| Block force push | yes |
| Require signed commits | yes |
| Enforce settings for admins | yes |

### [Dangerous Action Workflows](dangerous_workflows.yaml)

Leverages [Scorecard](#scorecard) to detect dangerous
GitHub Action use.

### [Outside collaborators](outside.yaml)

Controls how users outside of the organization can interact with repositories.

| | |
| - | - |
| Outside collaborators can have push access | false |
| Repos with no admins allowed? | false |

### [Scorecard](scorecard.yaml)

Runs [Scorecard](https://github.com/ossf/scorecard/) to detect and report a
wide variety of problems. See the [default checks.yaml](https://github.com/ossf/scorecard/blob/main/docs/checks/internal/checks.yaml)
for current settings.

## [SECURITY.md check](security.yaml)

Each repository is required to have a security policy published as `SECURITY.md`.
GSA developed open source software should be covered by the
[GSA Vulnerability Disclosure Policy](https://gsa.gov/vulnerability-disclosure-policy).

In most cases you should be able to use [SECURITY.md](./SECURITY.md) from this
repo.
