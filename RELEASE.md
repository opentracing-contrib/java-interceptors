# Release Process

This project uses the regular Gradle mechanics to publish artifacts. The release is automated via GitHub Workflows but a manual release can also be triggered.

## Automated

1. Tag the release: `git tag v0.1.1`
1. Push the tag to the main repository: `git push git@github.com:opentracing-contrib/java-interceptors.git v0.1.1`

Once this is done, GitHub will trigger a release, uploading it to Maven Central and GitHub Packages for this repository.

## Manual

To do a manual release, you'll first need a few environment variables:

* `GITHUB_ACTOR`, set to your GitHub username, required to publish as a GitHub package
* `GITHUB_TOKEN` (with `write:packages` permission only), required to publish as a GitHub package
* `ORG_GRADLE_PROJECT_sonatypeUsername`, required to publish to Maven Central
* `ORG_GRADLE_PROJECT_sonatypePassword`, required to publish to Maven Central
* `ORG_GRADLE_PROJECT_signingKey`, required to publish to Maven Central
* `ORG_GRADLE_PROJECT_signingPassword`, required to publish to Maven Central

The signing key can be set as follows. You'll need the key's password (passphrase).

```console
export ORG_GRADLE_PROJECT_signingKey="$(gpg -a --export-secret-key 08CBF182F1E7BB0F)" 
```

Do everything like the "Automated" session but instead of pushing the tag to the remote repository, run:

```bash
git checkout v0.1.1
./gradlew publish
```

