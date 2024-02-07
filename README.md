# Secrets and SAST scanning of publicly-available APKs

## Related Links

Google Drive folder for presentation/non-SCM files: https://drive.google.com/drive/folders/1Po1YpPNEcthpGrT-sxIwqHcVqTR_IHMb?usp=drive_link

## Tasks

1. Review Decompiled APK from Henry

## Goals

Determine via random sampling, what percentage of APKs on the Google Play store have exposed secrets in them. Additionally, SAST scans can be performed and API URLs can be discovered.

## Overview

1. Download an `.apk` file from Google Play
2. Decompile it with `apktool`
3. Scan it with a secret scanning tool, review results and pentest further
4. Scan it with a SAST tool, review results
5. Search for API URLs, review results

## Decompiled APKs

- To be built out. Put Git repositories containing decompiled APK source code here.

## Tools

### SAST

- Snyk

### Secrets Scanning

- gitLeaks
