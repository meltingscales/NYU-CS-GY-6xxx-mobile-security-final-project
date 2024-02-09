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

1.  "Student Portal"
    - https://play.google.com/store/apps/details?id=com.edu_pro.studentportal
    - https://github.com/meltingscales/com.edu_pro.studentportal
    - Summary: No findings.

2.  "Q StudentConnection"
    - https://play.google.com/store/apps/details?id=com.AequitasSolutions.StudentPortal
    - https://github.com/meltingscales/com.AequitasSolutions.StudentPortal
    - Summary: PENDING

3.  "Axis Mobile - Corporate"
    - https://play.google.com/store/apps/details?id=com.axisidp.mobile
    - https://github.com/meltingscales/com.axisidp.mobile
    - Summary:
      - 2 generic API keys exposed
      - 1 Google Maps API key exposed
      - The domain `idpm.axisbank.co.in` should have `nmap` and `dirb` ran on it. Running some HTTP service in India.

4.  "BHIM Axis Pay:UPI,Online Recha"
    - https://play.google.com/store/apps/details?id=com.upi.axispay
    - github link tbd - henry

5.  "PrismHR Employee Portal"
    - https://play.google.com/store/apps/details?id=com.prismhr.employeeportal
    - github link tbd - henry

6.  ClientiApp - Client management
    - https://play.google.com/store/apps/details?id=com.gg.clienti&hl=en
    - github link tbd - michael

7.  AppFolio Property Manager
    - https://play.google.com/store/apps/details?id=com.appfolio.appfolio_property_manager&hl=en
    - github link tbd - michael

8.  InteliChart Patient Portal
    - https://play.google.com/store/apps/details?id=ic.mobile.patientportal&hl=en
    - github link tbd - michael

9.  Verizon Business Group Network Vendor Portal
    - https://play.google.com/store/apps/details?id=raps.verizon.com.oneapplaunchersso&hl=en
    - github link tbd - michael

10. Paycom Software, Inc. Paycom
    - https://play.google.com/store/apps/details?id=com.paycom.mobile.ess&hl=en
    - github link tbd - michael


## Resources

- https://book.hacktricks.xyz/mobile-pentesting/android-app-pentesting
- https://github.com/ndelphit/apkurlgrep/issues/13
- Downloading APK files from an android device: https://stackoverflow.com/questions/4032960/how-do-i-get-an-apk-file-from-an-android-device

## Tools

### Decompilation

- [apktool](https://apktool.org/docs/install/)

### URLs

- [apkurlgrep](https://github.com/ndelphit/apkurlgrep)

### SAST

- [Snyk](https://app.snyk.io/login)

### Secrets Scanning

- [gitLeaks](https://gitleaks.io/)
