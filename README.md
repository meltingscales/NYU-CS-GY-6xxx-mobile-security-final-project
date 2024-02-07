# Secrets and SAST scanning of publicly-available APKs

## Related Links

Google Drive folder for presentation/non-SCM files: https://drive.google.com/drive/folders/1Po1YpPNEcthpGrT-sxIwqHcVqTR_IHMb?usp=sharing

## Tasks

1. Review Decompiled APK from Henry

## Goals

Determine what percentage of APKs on the Google Play store have exposed secrets in them. 

Additionally, SAST scans can be performed and API URLs can be discovered.

We can target small organizations' applications as their security may be worse than larger ones.

## Overview

1. Download an `.apk` file from Google Play
2. Decompile it with `apktool`
3. Scan it with a secret scanning tool, review results and pentest further
4. Scan it with a SAST tool, review results
5. Search for API URLs, review results

## Decompiled APKs

- "Student Portal"
  - https://play.google.com/store/apps/details?id=com.edu_pro.studentportal
- "Q StudentConnection"
  - https://play.google.com/store/apps/details?id=com.AequitasSolutions.StudentPortal
- 3
- 4
- 5
- 6
- 7
- 8
- 9
- 10

## Tools

### SAST

- Snyk

### Secrets Scanning

- gitLeaks
