# Release Process

This project uses the regular Gradle mechanics to publish artifacts. The release is automated via GitHub Workflows but a manual release can also be triggered.

## Automated

1. Tag the release: `git tag v0.1.1`
1. Push the tag to the main repository: `git push git@github.com:opentracing-contrib/java-interceptors.git v0.1.1`

Once this is done, GitHub will trigger a release, uploading it to Sonatype OSS Nexus instance (Maven Central). The automated release will not attempt to close nor release the staging repository created at Sonatype's OSS Nexus instance. Once a release is successful, make sure to close and release the staging repository.

## Manual

To do a manual release, you'll first need a few environment variables:

* `ORG_GRADLE_PROJECT_sonatypeUsername`
* `ORG_GRADLE_PROJECT_sonatypePassword`
* `ORG_GRADLE_PROJECT_signingKey`
* `ORG_GRADLE_PROJECT_signingPassword`

The signing key can be set as follows. You'll need the key's password (passphrase).

```console
export ORG_GRADLE_PROJECT_signingKey="$(gpg -a --export-secret-key 08CBF182F1E7BB0F)" 
```

Do everything like the "Automated" session but instead of pushing the tag to the remote repository, run:

```bash
git checkout v0.1.1
./gradlew publish
```

Similar to the automated release, this will not close nor release the staging repository. You'll need to do it manually.

## Sonatype OSS Nexus

Publishing to Sonatype OSS Nexus via Gradle often fails, as Nexus will create a staging repository when the first file is uploaded. As this might take a while, Gradle might think it timed out and retry the request, triggering the creation of another staging repository. Once that happens, you'll end up with two repositories: one probably containing only one file, and another containing all files (including the one from the first repository). In that case, drop one of the repositories and re-run the `publish` Gradle command, just to be safe. This time, the existing repository should be used. For the automated release, it's safe to re-run the release workflow once only one of the staging repositories is available.
