# Signing

Debug builds in these workflows do not sign a release. `assembleDebug` uses the debug keystore that the Android Gradle Plugin creates on the runner. Nothing in this repository stores a keystore, a store password, or a key password.

## Optional local release signing

Keep the keystore outside the repository. A typical local file is `keystore.properties` in the project root, listed in `.gitignore`:

```properties
storeFile=/absolute/path/outside/the/repo/release.jks
storePassword=
keyAlias=
keyPassword=
```

Leave the password fields empty in any example you commit. Fill them only on the machine that builds the release. Do not commit the keystore, and do not put the passwords in a workflow file.

## GitHub Actions

These reusable workflows do not accept signing secrets and do not upload release artifacts. A caller that adds release signing has to declare its own secrets and a separate job. That job is outside this blueprint. Do not grant the workflows here `contents: write` or `id-token: write` in order to publish a build.
