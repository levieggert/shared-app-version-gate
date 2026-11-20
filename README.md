# shared-app-version-gate
Stores versioning to enforce app updates.

Files are served via GitHub Pages:

```
https://<user>.github.io/shared-app-version-gate/<app>/version.json
https://<user>.github.io/shared-app-version-gate/<app>/version-staging.json
```

## Adding an app
Copy `template-app/` to a new folder named after the app and fill in the values.

## Schema
| Field | Meaning |
| --- | --- |
| `schema_version` | Format version of this file. Bump only on breaking changes to the structure. |
| `<platform>.required_build_number` | Integer build number (iOS `CFBundleVersion`, Android `versionCode`). Installed builds **below** this must update (blocking prompt). |
| `<platform>.latest_build` | Integer build number of the latest store release. Installed builds below this get an optional "update available" prompt. |
| `<platform>.store_url` | Where the update button sends the user. |
| `<platform>.force_update_message_localized_key` | Localization key the app looks up in its own strings to get the message shown on the forced-update prompt. |
| `<platform>.legacy_devices.max_supported_os_major` | Highest OS major version (e.g. iOS 15, Android 15) treated as a legacy device. Devices on this OS major or lower are "legacy". |
| `<platform>.legacy_devices.bypass_force_update` | If `true`, legacy devices skip the forced update, since the newer build may not support their OS and they'd otherwise be locked out. |

## Notes
- GitHub Pages caches responses for ~10 minutes, so a change can take that long to reach clients.
- Apps should **fail open**: if the file can't be fetched or parsed, let the user in.
- Only raise `required_build_number` after the new build is live in the store; otherwise users get stuck with nothing to update to.
