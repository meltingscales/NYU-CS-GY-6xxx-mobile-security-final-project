# Secrets and SAST scanning of publicly-available APKs

## Related Links

Google Drive folder for presentation/non-SCM files: https://drive.google.com/drive/folders/1Po1YpPNEcthpGrT-sxIwqHcVqTR_IHMb?usp=sharing

## Tasks

- Search for 5 APKs
  - Decompile them and put their decompiled source code into git repos, and update this document with links to the repos

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

## Search methodology

We want to target applications that are developed by smaller companies which may have more lax security standards. Sample search terms are below:

- Company portal 
- Student portal 
- Employee portal
- Healthcare
- HR portal 
- Accounting 
- Financial management 
- Inventory management 
- Point of sale (POS) 
- Time tracking 
- Task management 

## Decompiled APKs

- "Student Portal"
  - https://play.google.com/store/apps/details?id=com.edu_pro.studentportal
  - https://github.com/meltingscales/com.edu_pro.studentportal
- "Q StudentConnection"
  - https://play.google.com/store/apps/details?id=com.AequitasSolutions.StudentPortal
  - github link tbd
- "Axis Mobile - Corporate"
  - https://play.google.com/store/apps/details?id=com.axisidp.mobile
  - github link tbd
- "BHIM Axis Pay:UPI,Online Recha"
  - https://play.google.com/store/apps/details?id=com.upi.axispay
  - github link tbd
- "PrismHR Employee Portal"
  - https://play.google.com/store/apps/details?id=com.prismhr.employeeportal
  - github link tbd
- 6
- 7
- 8
- 9
- 10

## Tools

### URLs

- https://github.com/ndelphit/apkurlgrep

### SAST

- Snyk

### Secrets Scanning

- gitLeaks
