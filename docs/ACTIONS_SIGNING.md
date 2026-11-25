# GitHub Actions signing

The workflow builds the HAP, signs it with the SDK's `hap-sign-tool`, and uploads only `entry-default-signed.hap`. It is manually triggered and requires these repository Actions secrets:

| Secret | Value |
| --- | --- |
| `OHOS_STORE_BASE64` | Base64-encoded HarmonyOS `.p12` keystore |
| `OHOS_STORE_PASSWORD` | Keystore password |
| `OHOS_KEY_ALIAS` | Private-key alias in the keystore |
| `OHOS_KEY_PASSWORD` | Private-key password |
| `OHOS_PROFILE_BASE64` | Base64-encoded `.p7b` provisioning profile |
| `OHOS_CERT_BASE64` | Base64-encoded `.cer` certificate |

Create a matching HarmonyOS **debug/test** signing set for bundle name `com.weiyingshi.player`. For direct installation on a device, the debug profile must include that device's UDID. Add the values under **Settings > Secrets and variables > Actions** in the GitHub repository; do not commit signing files or passwords.

In Windows PowerShell, encode each file before adding it as a secret:

```powershell
[Convert]::ToBase64String([IO.File]::ReadAllBytes("release.p12"))
[Convert]::ToBase64String([IO.File]::ReadAllBytes("profile.p7b"))
[Convert]::ToBase64String([IO.File]::ReadAllBytes("release.cer"))
```

Generate the debug signing set in DevEco Studio using Huawei account automatic signing, or use a matching debug certificate/profile issued for the app and device. A standalone random keystore is not enough for device installation. After all six secrets are configured, run **Build HarmonyOS HAP** from the Actions tab. The workflow fails before SDK setup if any secret is missing and uploads only signed HAP output.
