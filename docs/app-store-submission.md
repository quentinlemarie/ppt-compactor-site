---
layout: default
title: App Store Submission Checklist
---

# App Store Submission Checklist

This is the working checklist for submitting PPT Compactor V1 to App Store Connect.

## Identifiers

- Bundle ID: `com.quentinlemarie.PPTCompactor`
- Product name: `PPT Compactor`
- Platform: macOS
- Category: Productivity
- Minimum macOS: 13.0

## Required App Store Connect setup

Create the app record manually in App Store Connect before running Fastlane upload lanes.

Required values:

- SKU: `ppt-compactor-mac`
- Bundle ID: `com.quentinlemarie.PPTCompactor`
- Primary language: English
- Age rating: no restricted content
- Pricing: set manually in App Store Connect

## App Store Connect copy

Paste these values into App Store Connect if entering metadata manually. The source files for Fastlane live under `fastlane/metadata/en-US/`.

App name:

```text
PPT Compactor
```

Subtitle:

```text
Clean oversized .pptx files
```

Promotional text:

```text
Shrink bulky .pptx files on your Mac by compacting large slide images, removing unused media, and converting embedded video or GIF files to still frames.
```

Description:

```text
PPT Compactor helps shrink oversized .pptx files before you send, upload, or archive them.

Open a .pptx file, review every embedded media item, and save a cleaned copy. The app highlights unused media, shows file sizes, and identifies where media is used in the deck, including whether an item is inside or outside the visible slide boundary.

What it can do:

- remove unused embedded media
- convert embedded videos to still frames
- convert animated GIFs to still PNG images
- keep used media safe by default
- export or copy selected media files
- preserve your original file by writing a cleaned copy

PPT Compactor runs locally on your Mac. It does not upload your presentations, does not require an account, and does not use analytics.
```

Keywords:

```text
pptx,compress,slides,media,video,gif,cleanup,presentation,mac,deck
```

Release notes:

```text
Initial Mac release with local .pptx media inspection, unused-media cleanup, video and GIF still-frame conversion, and before/after savings review before export.
```

URLs:

- Marketing URL: `https://quentinlemarie.github.io/ppt-compactor-site/`
- Support URL: `https://quentinlemarie.github.io/ppt-compactor-site/support/`
- Privacy URL: `https://quentinlemarie.github.io/ppt-compactor-site/privacy/`

Copyright:

```text
Copyright 2026 Quentin Lemarie
```

## Local validation

Run:

```bash
bash scripts/asc-preflight.sh
xcodebuild test -project PPTCompactor.xcodeproj -scheme PPTCompactor -destination 'platform=macOS'
```

## Screenshots

Use:

```bash
bash scripts/capture-app-store-screenshots.sh 01-main-inventory
```

Prepare the app window first, then click the window when prompted. Store final images under:

- `marketing/screenshots/`
- `fastlane/screenshots/en-US/`

Recommended screenshot set:

- empty drop zone
- media inventory with filters visible
- detail pane showing usage and slide boundary status
- done overlay with before/after savings

## Fastlane environment

Fastlane uses App Store Connect API key auth. Preferred setup reads the key from Google Secret Manager in `slideforge-prod-2026`. The secret may be JSON with `key_id`, `issuer_id`, and `key_content`, or raw `.p8` content with key metadata supplied by environment variables.

Required for upload lanes:

```bash
export APPLE_ID="quentin.lemarie@gmail.com"
export FASTLANE_TEAM_ID="<developer-portal-team-id>"
export FASTLANE_ITC_TEAM_ID="<app-store-connect-team-id>"
export ASC_API_KEY_SECRET="ppt-compactor-asc-api-key"
export ASC_GCLOUD_PROJECT="slideforge-prod-2026"
```

Local fallback:

```bash
export ASC_KEY_ID="<app-store-connect-api-key-id>"
export ASC_ISSUER_ID="<app-store-connect-issuer-id>"
export ASC_KEY_PATH="$HOME/.private_keys/AuthKey_<key-id>.p8"
```

`ASC_KEY_CONTENT` may be used instead of `ASC_KEY_PATH` in CI. Do not commit API keys.

## Upload order

1. `bundle exec fastlane mac preflight`
2. `bundle exec fastlane mac upload_metadata`
3. `bundle exec fastlane mac build_mas`
4. `bundle exec fastlane mac upload_build`

The lanes do not submit for review. Submit manually after reviewing screenshots, privacy, pricing, and export compliance in App Store Connect.
