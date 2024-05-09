# GSA-TTS AllStar Administration

This document is for owners of this (`.allstar`) repository and the GSA-TTS org.

Developers working in the GSA-TTS org who wish to leverage AllStar should
check our [README.md](./README.md).

## GitHub App for AllStar - gsa-tts-allstar

An "App" is like a service account: It is a user-like thing with a set of
permissions in your GitHub organization. Private key authentication can be used
to allow a GitHub Action (or anything) to authenticate as the "App".

See [AllStar - Operator - Create a GitHub App](https://github.com/ossf/allstar/blob/main/operator.md#create-a-github-app)
for details on how to create a new App. Note the following important differences
in our implementation:

* We ONLY allow write access to `Issues` at this time. (Auto-remediation is off.)
* Repository Access is opt-in only and the list of repositories that the `gsa-tts-allstar`
  App can manage is explicitly defined. (See [Giving AllStar Repository Access](#giving-the-allstar-app-repository-access))
* When you create the `gsa-tts-allstar` app user make sure to record the `App ID` value

### gsa-tts-allstar private key

To generate or replace the private key used by the AllStar action to authenticate
with GitHub:
* Navigate to https://github.com/organizations/GSA-TTS/settings/apps/gsa-tts-allstar
* Under "Private keys" click "Generate a private key"
* Replace the GitHub Actions environment secret with the contents of the new one (See [Managing the Prod Deployment Environment](#managing-the-prod-deployment-environment))
* Do not leave this key unprotected! Delete it immediately after updating
  the GitHub Actions environment secret.

## Giving the AllStar App Repository Access

__Do not give AllStar full organization access!__

To add a new opt-in repo:
* As a [GSA-TTS owner](https://github.com/orgs/GSA-TTS/people?query=role%3Aowner),
  navigate to [Settings -> GitHub Apps -> gsa-tts-allstar](https://github.com/organizations/GSA-TTS/settings/installations/40988991)
* Under "Repository access" use the "Select repositories" drop down to add
  repos for AllStar to monitor
* Click Save

## AllStar GitHub Action

The AllStar GitHub Action requires some setup before it is usable in a new
organization.

### Managing the prod deployment environment

To protect secrets we utilize the deployment environment feature of GitHub
Actions.

* Under [Settings -> Environments](https://github.com/GSA-TTS/.allstar/settings/environments)
  create the `prod` environment (if not already defined)
* Uncheck "Allow administrators to bypass configured protection rules"
* Under "Deployment branches" switch to "Selected Branches"
* Click "Add deployment branch rule" and enter `main` then click "Add rule"
* Under "Environment variables" click "Add variable"
  * Name: `APP_ID`
  * Value: Enter the App ID for the `gsa-tts-allstar` app user (from [gsa-tts-allstar private key](#gsa-tts-allstar-private-key))
  * Click "Add variable" to complete
* Under "Environment secrets" click "Add secret"
  * Name: `PRIVATE_KEY`
  * Value: Paste the contents of the private key PEM downloaded in [gsa-tts-allstar private key](#gsa-tts-allstar-private-key)
  * Click "Add secret" to complete
* From this point, future AllStar GitHub Action runs on `main` should function.

### Updating the version of AllStar image used

The AllStar project publishes new container images as part of each release.
These are available from the [allstar container repository](https://github.com/ossf/allstar/pkgs/container/allstar/versions?filters%5Bversion_type%5D=tagged).

To update:
* Open a PR to update [.github/workflows/allstar-run.yml](.github/workflows/allstar-run.yml) with the new
  tag value. Look for the following line and make sure to use the `-busybox` image.
  ~~~yaml
  container: ghcr.io/ossf/allstar:{TAG}-busybox
  ~~~

  For example, if the release is `v7.1`:
  ~~~yaml
  container: ghcr.io/ossf/allstar:v7.1-busybox
  ~~~
* Once reviewed and merged make sure to monitor the action under https://github.com/GSA-TTS/.allstar/actions
  and address any issues.


