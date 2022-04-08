---
title: How to open a Mac app from an unidentified developer
date: 2021-12-19 18:27:09
keywords: Mac OS
categories: Mac OS
tags:
  - Shell
---

![macbook_air](https://s2.loli.net/2021/12/19/hmrwIpOfZGygvtH.png)

Some complain about Apple's walled garden, or if you prefer a less flowery term: closed platform. Apple would say that maintaining a level of control over the apps we can install on our devices protects us from malware and a bad user experience, but it can be frustrating and worrying if you want to run an app and you are confronted by a warning that it is from an unidentified developer.

Luckily it is possible to open and run these apps and we will show you how. But before you do so be warned: do this only if you are satisfied that the developer and software (and the means of distribution, since innocent apps can be hijacked by guilty parties) are legit. We discuss the safety of unidentified apps later in this article.

#### Why am I seeing an unidentified developer warning?

Apple has a lot of control over the apps available for Macs, iPads and iPhones. While the Mac is a little more open than iOS - the only way to get third party apps onto your iPhone and iPad is to download them from the iOS App Store - there are still a lot of hoops to jump through before you can install and run some third party apps on your Mac.

As we said above there is good reason for this. These measures are designed to protect us from malware that might arrive on our Macs disguised as an app that we think we can trust. It might even look like a well-known app but have malicious code added to it. While we can all follow the advice not to download apps from file-sharing sites, or via links on dodgy looking emails, Apple's basically put in measures to make it harder for us to install apps that might be dangerous.

These measures include Gatekeeper, which is Apple's name for the security aspect of macOS that checks apps for malware and quaranteens them. It also checks whether the app is written by a developer known to Apple (aka signed). Then, even if it matches those requirements, Gatekeeper will ask you to confirm that you want to open the app. In macOS Catalina, which was introduced in October 2019, Apple made Gatekeeper even more stringent. Previously you could get around Gatekeeper by launching the app via Terminal but now if you open an app via Terminal Gatekeeper will still check it out. Another change is that Gatekeeper will run its list of checks everytime you open an app.

So, how can you open apps from unidentified developers? And how can you stop seeing the warning everytime you open an app?

#### How to open apps not from Mac App Store

By default macOS allows you to open apps from the official App Store only. If you have this still set as your default you will be seeing the warning when you try to open an app for the first time.

Luckily you can make a simple change to your settings that will allow you to open some third-party apps that aren't on the App Store. It won't mean that you can open every third party app without issue, but it will certainly mean you see fewer warnings.

1. Open System Preferences.
2. Go to the Security & Privacy tab.
3. Click on the lockand enter your password so you can make changes.
4. Change the setting for 'Allow apps downloaded from' to 'App Store and identified developers' from just App Store.

![system_prefs_open_apps](https://s2.loli.net/2021/12/19/b4wLS3l7kFyDrpu.png)

You'll still be prevented from opening anything macOS doesn't recognise, but at least you will be able to open apps that weren't purchased from the App Store, assuming that they don't have malware and they are signed by a developer Apple recognises and trusts.

#### How to open a blocked app

If you attempt to open an app and macOS stops you from doing so, that doesn't necessarily mean there is something wrong with the app. But it will indicate that the app isn't from an 'identified developer' - in other words a developer that has signed up to Apple's developer program and jumped through a few hoops to get Apple to trust it.

Luckily you can still open the app and override the block. Here's how:

1. Open System Preferences.
2. Go to Security & Privacy and select the General tab.

![how_to_open_mac_app_unidentified_developer](https://s2.loli.net/2021/12/19/lzqYt5UdBbZ94Hg.jpg)

3. If you've been blocked from opening an app within the past hour, this page will give you the option to override this by clicking the temporary button 'Open Anyway'.
4. You'll be asked one more time if you're sure, but clicking Open will run the app.

![how_to_open_mac_app_unidentified_developer_532b](https://s2.loli.net/2021/12/19/UF9MGk5P6g1DjOi.jpg)

This creates an exception for that app, so you'll also be able to open it in the future without having to repeat this process.

Because of Gatekeeper's other checks this will still stop you from opening an app with known malware attached to it.

#### Other ways to open blocked apps

Another way to open a blocked app is to locate the app in a Finder window. 

1. Open the Finder.
2. Locate the app (it might be in the Applications folder, or it might still be in your downloads folder).
3. Ctrl-Click or right-click on the app.

![open_libraoffice](https://s2.loli.net/2021/12/19/ZRGFdpOIkcbtgLT.png)

4. Select Open from the resultant menu and the app will be opened anyway, and an exception will be created for opening it normally (i.e. by double-clicking) in future.

#### How to 'Allow Apps from Anywhere'

As you can see above, the Security & Privacy section of System Preferences presents you with two settings for the types of apps you allow to run: ones from the App Store, or ones from the App Store or identified developers.

![how_to_allow_apps_from_anywhere_mac](https://s2.loli.net/2021/12/19/Em5RsM4qocSH3vC.jpg)

But there is a third, hidden option: 'Allow apps from anywhere'. This used to be an option in earlier versions of macOS, but disappeared when macOS Sierra arrived. However you can get the Anywhere option back.

We will say right away that we don't recommend this setting, which puts you at risk of installing malware under the guise of legitimate software. But if you are determined on this course, it's possible to make that option reappear with a line of code in Terminal.

Open Terminal and enter the following code to get your Anywhere option:

```bash
sudo spctl --master-disable
```

Now press Return, and you will be asked to enter your password. Once that's done, open System Preferences (if it's already open, you'll need to quit it and restart to see the new options) and go to the Security & Privacy section.

![how_to_allow_apps_from_anywhere_mac](https://s2.loli.net/2021/12/19/xajiu8qvG4XEHkl.png)

A new, third option will have appeared allowing you to 'Allow apps downloaded from: Anywhere'. You'll have to click the padlock icon to make changes to the settings on this page.

#### How to remove the 'Anywhere' option

If you share your Mac with someone else it might be wise to get rid of the Anywhere option. To hide it again, you'll need to go to Terminal again, and this time type:

```bash
sudo spctl --master-enable
```

#### Is it safe to open unidentified apps?

It might be, it might not. The point is that you don't have Apple's certification that it is, so you will have to rely on your own due diligence to ensure that the software is okay.

Before installing the software you should search for reviews of the app, information about the company (and distribution site/platform), and advice and testimonials from other users. Bear in mind as ever that dodgy companies are not above planting a few fake reviews to give themselves the sheen of legitimacy, so keep searching after the first few results. If you're not satisfied, it may be safer to find an alternative that macOS is happier to install.

When installing unidentified apps you should also make extra-sure that your anti virus software is up to date.

Note that getting the 'unidentified developer' warning dialog doesn't mean you're about to install some malware. As Apple acknowledges, there are plenty of reasons why a perfectly legitimate company might not be on the identified list; it might for instance be that the app is older than the company's developer registration programme.
