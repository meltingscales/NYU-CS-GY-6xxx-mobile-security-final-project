# Secrets and SAST scanning of publicly-available APKs

## Related Links

- Google Drive folder for presentation/non-SCM files: https://drive.google.com/drive/folders/1Po1YpPNEcthpGrT-sxIwqHcVqTR_IHMb?usp=sharing
- Youtube Video: https://youtu.be/4o2URWPtqX4


## Tasks

- Search for 5 APKs
  - Decompile them and put their decompiled source code into git repos, and update this document with links to the repos
- Add 10 APKs from a formalized list
  - See <https://www.makeuseof.com/tag/most-popular-android-apps/#:~:text=The%2020%20Most%20Popular%20Android%20Apps%20in%20the,Netflix%20(%2B9)%201.88%20billion%20downloads%20...%20More%20items> and pick the top ten

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
    - Status: Done.
    - https://play.google.com/store/apps/details?id=com.edu_pro.studentportal
    - https://github.com/meltingscales/com.edu_pro.studentportal
    - Summary: No findings.

2.  "Q StudentConnection"
    - Status: Done.
    - https://play.google.com/store/apps/details?id=com.AequitasSolutions.StudentPortal
    - https://github.com/meltingscales/com.AequitasSolutions.StudentPortal
    - Summary: No findings.

3.  "Axis Mobile - Corporate"
    - Status: Done.
    - https://play.google.com/store/apps/details?id=com.axisidp.mobile
    - https://github.com/meltingscales/com.axisidp.mobile
    - Summary:
      - 2 generic API keys exposed
      - 1 Google Maps API key exposed
      - The domain `idpm.axisbank.co.in` should have `nmap` and `dirb` ran on it. Running some HTTP service in India.
      - Snyk SAST:
        - Hardcoded IVs for crypto keys,
        - They use DESede ECB with no padding,
        - They may have XXE, by parsing XML unsafely,
        - They use AES CBC,
        - Assorted Medium findings.

4.  "BHIM Axis Pay:UPI,Online Recha"
    - Status: Done.
    - https://play.google.com/store/apps/details?id=com.upi.axispay
    - https://github.com/meltingscales/com.upi.axispay
    - Summary:
      - No leaked secrets.
      - apkurlgrep: 
        - Looks like they use DESede and AES CBC for some reason.
        - A LOT of URLs show up. At least 100 endpoints. They're probably related to <https://upiuat.axisbank.co.in/v1/>. I hope this company has a good API security program in place!
          - /v1/bank/transactions/pay
          - /v1/customer/accounts
          - /v1/customer/accounts/mobreg
          - /v1/customer/otp
          - /v1/customer/accounts/remove
          - /v1/customer/accounts/update
          - /v1/authenticate
          - /v1/bank/transactions/balanceinquiry/creditline
          - ...etc
      - Snyk SAST:
        - Hardcoded IVs for encryption,
        - Uses Random.nextInt for seeding encryption
        - Deserializes serialized untrusted data
        - A handful of Medium findings.


5.  "PrismHR Employee Portal"
    - Status: DONE
    - https://play.google.com/store/apps/details?id=com.prismhr.employeeportal
    - https://github.com/meltingscales/com.prismhr.employeeportal
    - Summary:
      - apkurlgrep:
        - Found 1 URL: https://epycorp-ep.prismhr.com/apis/ep/peos?fwdClientCode=
          - Seems to be used with a URL parameter. We could probably fuzz the `fwdClientCode` parameter.

6.  ClientiApp - Client management
    - Status: DONE
    - https://play.google.com/store/apps/details?id=com.gg.clienti&hl=en
    - github https://github.com/maa9605/clientapp
    - Summary:
      - Snyk SAST:
          - Use of a hardcoded array for crypto keys,
          - IV parameter using hardcoded array,
          - Use of SHA-1
          - Android Broadcast without
          
7.  AppFolio Property Manager
    - Status: DONE
    - https://play.google.com/store/apps/details?id=com.appfolio.appfolio_property_manager&hl=en
    - github https://github.com/maa9605/appfolio
    - Summary:
      - Snyk SAST:
          - Possible TLS vulnerability
          - Unsanitized URI input (Possible SQL injection vulnerability)  

8.  InteliChart Patient Portal
    - Status: DONE
    - https://play.google.com/store/apps/details?id=ic.mobile.patientportal&hl=en
    - github https://github.com/maa9605/patientportal
    - Summary:
      - Snyk SAST:
          - Possible TLS vulnerability
          - Unsanitized URI input (Possible SQL injection vulnerability)
          - Unsecured communication with local port 
          
9.  Verizon Business Group Network Vendor Portal
    - Status: DONE
    - https://play.google.com/store/apps/details?id=raps.verizon.com.oneapplaunchersso&hl=en
    - github https://github.com/maa9605/vendorapp
    - Summary: No Findings of importance

10. Paycom Software, Inc. Paycom
    - Status: DONE
    - https://play.google.com/store/apps/details?id=com.paycom.mobile.ess&hl=en
    - github https://github.com/maa9605/paycom
    - Summary: No Findings of importance

11. WhatsApp (Michael)
    -  Status: DONE
    -  https://play.google.com/store/apps/details?id=com.paycom.mobile.ess&hl=en
    -  github https://github.com/maa9605/whtsapp
    -  Summary: 1 Critical Issue Found
      -  Google API Key Found: resources/res/values/strings.xml

13. Facebook (Michael)
    -  Status: DONE
    -  https://play.google.com/store/apps/details?id=com.paycom.mobile.ess&hl=en
    -  github https://github.com/maa9605/whtsapp
    - Summary: 4 Critical Issues Found
      -  Username & Password Found in File: sources/org/webrtc/PeerConnection.java
      -  Generic Password: sources/com/facebook/graphql/enums/GraphQLAllPaymentTypeFieldsSet.java
      -  Google API Key Found: sources/X/C32150G3b.java
      -  Facebook App Key Found: sources/X/C28211et.java


15. Messenger (Michael)
    -  Status: DONE
    -  https://play.google.com/store/apps/details?id=com.paycom.mobile.ess&hl=en
    -  github https://github.com/maa9605/whtsapp
    -  Summary: 2 Critical Issues Found
      -  Google API Key Found: resources/AndroidManifest.xml
      -  Facebook App Key Found:sources/com/facebook/messaging/accountlogin
      ~ fragment/segue/AccountLoginSegueCredentials.java

17. TikTok (Michael)
    -  Status: DONE
    -  https://play.google.com/store/apps/details?id=com.paycom.mobile.ess&hl=en
    -  github https://github.com/maa9605/whtsapp
    -  Summary: 1 Critical Issue Found
      -  Google API Key Found: resources/AndroidManifest.xml

19. Instagram (Michael)
    -  Status: DONE
    -  https://play.google.com/store/apps/details?id=com.paycom.mobile.ess&hl=en
    -  github https://github.com/maa9605/whtsapp
    -  Summary: 3 Critical Issues Found
      -  Google API Key Found: resources/res/values/strings.xml
      -  Facebook App Key Found: sources/X/AbstractC19240xM.java
      -  Facebook App Key Found: sources/X/AnonymousClass224.java

21. Facebook Lite (Henry)
    - Status: DONE
    - https://play.google.com/store/apps/details?id=com.facebook.lite
    - github: https://github.com/meltingscales/com.facebook.lite
    - Summary:
      - Snyk SAST:
        - TLS 1.0 is used instead of TLS 1.2
      - apkurlgrep
        - Various API endpoints, but nothing really juicy.

22. SHAREit (Henry)
    - Status: WIP
    - https://play.google.com/store/apps/details?id=com.lenovo.anyshare.gps
    - github: https://github.com/meltingscales/com.lenovo.anyshare.gps
    - Summary:
      - gitleaks
        - Some GCP API keys may be exposed.
      - apkurlgrep
        - Some API URLS are revealed. Could be useful to probe these.
      - Snyk SAST
        - Currently scanning.
      - jadx
        - NSTR.

23. Netflix (Henry)
    - Status: WIP
    - https://play.google.com/store/apps/details?id=com.netflix.mediaclient
    - github: https://github.com/meltingscales/com.netflix.mediaclient
    - Summary: 
      - Snyk SAST
        - Currently scanning.

24. Snapchat (Henry)
    - Status: WIP
    - https://play.google.com/store/apps/details?id=com.snapchat.android
    - github: https://github.com/meltingscales/com.snapchat.android
    - Summary: 
      - Snyk SAST
        - Currently scanning.
      - gitleaks
        - 1 "SafetyNet" API key is exposed in the source code of a file called `BuildConfig.java`.
      - apkurlgrep
        - A lot of internal SnapChat API URLs and routes show up.

25. Telegram (Henry)
    - Status: DONE
    - https://play.google.com/store/apps/details?id=org.telegram.messenger
    - github: https://github.com/meltingscales/org.telegram.messenger
    - Summary:
      - Snyk SAST:
        - A few SQL injection vulnerabilities, use of AES CBC, and a few other mediums.
      - gitleaks
        - 1 "SafetyNet" API key is exposed in the source code of a file called "BuildVars.java"

## Resources

- https://book.hacktricks.xyz/mobile-pentesting/android-app-pentesting
- https://github.com/ndelphit/apkurlgrep/issues/13

## Tools

### Downloading APKs

- [Use adb](https://stackoverflow.com/questions/4032960/how-do-i-get-an-apk-file-from-an-android-device)
- [apkcombo.com](https://apkcombo.com/)

### Decompilation

- [apktool](https://apktool.org/docs/install/)
- [jadx](https://github.com/skylot/jadx)

### URLs

- [apkurlgrep](https://github.com/ndelphit/apkurlgrep)

### SAST

- [Snyk](https://app.snyk.io/login)

### Secrets Scanning

- [gitLeaks](https://gitleaks.io/)
