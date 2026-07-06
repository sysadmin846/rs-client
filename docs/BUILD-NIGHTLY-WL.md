# WL RustDesk Client Nightly Build

## Purpose

This build is for the Wangli remote-control client. It includes the desktop home-page controls for unauthenticated devices:

- `申请管理员远程`: submit a temporary remote request to the platform.
- `生成连接码`: create a 10-minute connection code and copy it to the clipboard.

After the target user completes DingTalk authorization, administrators can remote directly from the platform. If the target user has not completed DingTalk authorization, administrators must use the approved temporary request or the connection code flow.

## Required Platform APIs

The client uses the configured `api-server` and calls:

- `POST /api/remote_auth/client/request`
- `POST /api/remote_auth/client/code`

The platform gateway must also forward:

- `/api/remote_auth/*` to RustDesk API Server
- `/api/ticket/*` to RustDesk API Server

The wl57 platform has been updated for these routes.

## GitHub Actions Build

Repository:

```text
https://github.com/sysadmin846/rs-client
```

Manual workflow:

```text
Flutter Build (Manual)
.github/workflows/flutter-nightly.yml
```

The workflow delegates to `.github/workflows/flutter-build.yml`.

Recommended tag format:

```text
nightly-YYYYMMDD-HHMM-remote-auth
```

Example:

```bash
gh workflow run "Flutter Build (Manual)" \
  --repo sysadmin846/rs-client \
  --ref <branch-name> \
  -f upload-tag=nightly-20260706-remote-auth
```

Watch the run:

```bash
gh run watch <run-id> --repo sysadmin846/rs-client
```

List recent runs:

```bash
gh run list --repo sysadmin846/rs-client --workflow "Flutter Build (Manual)" --limit 10
```

## Outputs

When `UPLOAD_ARTIFACT=true`, the workflow publishes a prerelease. The release tag is:

```text
<upload-tag>-<github-run-number>
```

Windows x64 outputs:

- `rustdesk-1.4.4-x86_64.exe`
- `rustdesk-1.4.4-x86_64.msi`

macOS outputs, if the macOS jobs complete:

- `rustdesk-1.4.4-x86_64.dmg`
- `rustdesk-1.4.4-aarch64.dmg`

Download command:

```bash
gh release download <release-tag> \
  --repo sysadmin846/rs-client \
  --pattern "rustdesk-1.4.4-x86_64.*" \
  --dir ./build_artifacts/<release-tag>
```

## Installation Check

After installing the Windows package:

1. Open the RustDesk client.
2. Confirm the home page shows `申请管理员远程` and `生成连接码` below the local ID.
3. Click `申请管理员远程`; the platform `远程桌面 -> 授权管控 -> 待审批` should show a client request.
4. Click `生成连接码`; the code should be copied and should work in the platform authorization-control connection-code box for 10 minutes.

## Build Record: 2026-07-06

Source branch:

```text
remote-auth-client-build-20260706
```

Source commit:

```text
e74a7d21d371786425be7d21fe9289c373c17190
```

GitHub Actions run:

```text
https://github.com/sysadmin846/rs-client/actions/runs/28770856197
```

Run result:

```text
completed / success
```

Release:

```text
https://github.com/sysadmin846/rs-client/releases/tag/nightly-20260706-remote-auth-22
```

Published Windows x64 packages:

- `rustdesk-1.4.4-x86_64.exe`
- `rustdesk-1.4.4-x86_64.msi`

Published macOS packages:

- `rustdesk-1.4.4-x86_64-x86_64.dmg`
- `rustdesk-1.4.4-aarch64-aarch64.dmg`

Local download directory on the build operator machine:

```text
/Users/luwenji/Desktop/rs-client-nightly-remote-auth/nightly-20260706-remote-auth-22
```

Windows x64 SHA-256 checksums:

```text
5402e08617989a1f5b94af7826edae4121a8d0e0a2710e3b447ff829d2465a0f  rustdesk-1.4.4-x86_64.exe
36d220337d6b8a23a307961f31fc94f8d8e25d896247bbe32224e89944103098  rustdesk-1.4.4-x86_64.msi
```

macOS SHA-256 checksums:

```text
6c50d33858e1c8a9e0976dbead6e905a2d7b7e89f51e0089b26888da1140c95e  rustdesk-1.4.4-x86_64-x86_64.dmg
cd3d06d5b886563a4987f82c00381c27a23751f33e084bedaddc30c503b1dbde  rustdesk-1.4.4-aarch64-aarch64.dmg
```
