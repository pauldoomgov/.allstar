# Allstar configuration for GSA-TTS

![AllStar Run](https://github.com/GSA-TTS/.allstar/actions/workflows/allstar-run.yml/badge.svg)

[Allstar](https://github.com/ossf/allstar) is a security-policy GitHub app. It is
installed on this org, and this repo contains the configuration for that app. It
is configured to create issues on repos that do not comply with the configured
policy.

AllStar is set to "complain mode" at this time and can not auto-remediate issues.

## Enabled Repos

AllStar is currently configured to scan all the repositories which
it's authorized to access in the corresponding GitHub App, so repo updates
require the GSA TTS DevTools team to update those permissions.

If you have a repository in this organization that you would like added:

* Open an issue requesting which repositories we should add
* Once accepted and merged, check the `Issues` tab in your repository for AllStar's findings

### Overriding AllStar Defaults

Repository owners can [tune/override the organization-wide settings](https://github.com/ossf/allstar/tree/main?tab=readme-ov-file#repo-policy-configurations-in-the-org-repo) by creating
their own `.allstar/` directory in their repos.   E.g., `myrepo/.allstar/branch_protection.yaml` would override the [organization-wide branch protection configuration](./branch_protection.yaml).

### Compliance considerations

You can use AllStar to wholly or partially meet common 800-53 controls. These are described in the policies below.

## Policy Configuration

These are the expected settings to be in compliance

### [AllStar Main Config](allstar.yaml)

Sets the issue labels and issue footers for when AllStar creates issues.

### [Admin Requirements](admin.yaml)

|   |   |
| - | - |
| Owner-less repositories allowed | false |
| Individual users allowed to be admins | true |
| Maximum individual admins users | 4 |
| Teams allowed to be admins | true |
| Maximum admin teams | 2 |

**Compliance**: The repository admin/owner checks provide a partial implementation of:

* AC-5 Least Privilege
* AC-6 Separation of Duties

### [Binary Artifacts](binary_artifacts.yaml)

No binary files are currently ignored. You should override this policy
in your repository and set `ignoreFiles` to a list of the expected in-repo
binaries you wish to allow.

**Compliance**: By ensuring that all content in GitHub is reviewable, this provides a partial implementation of:

* SI-3: Malicious Code Protection

### [Branch Protection](branch_protection.yaml)

Sets baseline controls to ensure the change control process is followed
for code to reach `main`.

| | |
| - | - |
| Approvals required | 1 |
| Block force push | yes |
| Dismiss stale reviews | yes |
| Branches enforced | default |
| Enforce settings for admins | yes |
| OptOut on archived repos | yes |
| Require approval | yes |
| Require signed commits | yes |
| Require up-to-date branch before merge | yes |

**Compliance**:

* AC-2 Access Control:  AllStar is ensuring branch protection is being enforced and requires peer review by at least one other team member for the production “main/master” branch. Scans, checks, and branch protection policies are enforced configurations through the GSA-TTS Github Allstar implementation.
* SI-7 Software, Firmware, and Information Integrity: Signed commits ensure code updates come from the approved set of contributors.

### [Dangerous Action Workflows](dangerous_workflows.yaml)

Leverages [Scorecard](#scorecard) to detect dangerous
GitHub Action use.

### [Outside collaborators](outside.yaml)

Controls how users outside of the organization can interact with repositories.

| | |
| - | - |
| Outside collaborators can have push access | false |
| Repos with no admins allowed? | false |

**Compliance**:

* AC-3: Access Enforcement 
* AC-14: Permitted Actions Without Identification or Authentication

### [Scorecard](scorecard.yaml)

Runs [Scorecard](https://github.com/ossf/scorecard/) to detect and report a
wide variety of problems. See the [default checks.yaml](https://github.com/ossf/scorecard/blob/main/docs/checks/internal/checks.yaml)
for current settings.

As of May 2024, we have not enabled any default Scorecard checks across all repositories.

### [SECURITY.md check](security.yaml)

Each repository is required to have a security policy published as `SECURITY.md`.
GSA developed open source software should be covered by the
[GSA Vulnerability Disclosure Policy](https://gsa.gov/vulnerability-disclosure-policy).

In most cases you should be able to use [SECURITY.md](./SECURITY.md) from this
repo.

**Compliance**:

* RA-5(11): Vulnerability Monitoring and Scanning -- Public Disclosure Program

## Unimplemented check

We aren't using the "Github Actions" (`actions.yaml`) or "CODEOWNERS" (`codeowners.yaml`) since they're not well-enough documented upstream.

## AllStar Administration

See [ADMIN.md](./ADMIN.md) if you are a GSA-TTS administrator managing AllStar.

