---
title: How to install Kindle app on Windows 11
draft: false
tags:
  - reading
  - productivity
  - technology
time: 2025-02-28 23:08
---

## Steps

- (Shortcut) Download `APK File Installer for Windows` from the Microsoft Store and use it to install WSA.
- Download [Amazon Kindle 8.112.3.0(2.0.28400.0)](https://www.apkmirror.com/apk/amazon-mobile-llc/amazon-kindle/amazon-kindle-8-112-3-02-0-28400-0-release/amazon-kindle-8-112-3-02-0-28400-0-2-android-apk-download/download/?key=d7a991f006022e46ba9b608973abd9c37b9ccc8b). Newer versions may fail to install.
- Type `adb connect 127.0.0.1:58526` in the Command Prompt Window. Press Enter and wait for the connection.
- Once connected, type `adb install "name.apk"` in the Command Prompt. Replace name with the name of the apk file that you want to install and press Enter. After successful install, the installed App will appear in Windows Start Menu. Repeat this step for all the apk files that you want to install and they will appear in Windows Start Menu as well.

P.S. If the app is partially unable to access the Internet, the user can login through Amazon App Store to bypass the login process of the Kindle app. And the Kindle Scribe Notebooks can be accessed by:
> - In Chrome download "Advanced User-Agent" plugin and configure like the screenshot below. This will allow you to setup access to your notebooks. 
> - "User Agent": Mozilla/5.0 (iPad; CPU OS 13_2 like Mac OS X) AppleWebKit/605.1.15 (KHTML, like Gecko) CriOS/129.0.0.0 Mobile/15E148 Safari/604.1 (This simulates IPAD when going to this site)
> - "Run Value": https://read.amazon.com/kindle-notebook?ref_=neo_mm_yn_na_kfa
> ![image](https://gist.github.com/user-attachments/assets/a534fa43-6340-443d-9e2b-ba4fef1266f9)


## References
- https://gist.github.com/HimDek/e09340eae2861e1ad8b7f6bdba5ee9ff
- https://www.amazonforum.com/s/question/0D56Q0000Cvl66iSQA/how-to-view-kindle-scribe-notes-online