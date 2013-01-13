---
layout: post
title: Localize the Default splash screen image in iOS
published: true
comments: true
categories: iOS

---
  
[Apple Official Documentation:](href="http://developer.apple.com/library/IOS/#documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BuildTimeConfiguration/BuildTimeConfiguration.html)  
>  
An iOS application should be internationalized and have a language.lproj directory for each language it supports. In addition to providing localized versions of your application&rsquo;s custom resources, you can also localize your application icon, launch images, and Settings icon by placing files with the same name in your language-specific project directories. Even if you provide localized versions, however, include a default version of these files at the top level of your application bundle. The default version is used when a specific localization is not available.
  
Procedures:   
(note: "X" stands for either Landscape, Portrait, iPhone, iPad suffixes)     
  
1. Add the Default-X.png image on your Xcode project. I've put this file under the "Supporting Files" group. It should be on the top level of the application bundle
2. Click Default-X.png. Then, select the Identity inspector.
3. Select the drop down Localization tab. From there, you can select which language to localized for this image.
4. First, I've select English. Then, browse to the en.lproj folder put the correct English Default splash screen image
5. In my case I've localized it to Russian. Select again the localization tab and click "+", add the Russian language. Then, browse to the rus.lproj folder to put the correct Russian default spash screen image.   
  
Pls. do note Localized resources are placed in subdirectories with an ISO 639-1 language abbreviation for a name plus an .lproj suffix. (For example, the en.lproj, fr.lproj, and es.lproj directories contain resources localized for English, French, and Spanish.) For more information, see [Internationalizing Your Application.](http://developer.apple.com/library/IOS/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BuildTimeConfiguration/BuildTimeConfiguration.html#//apple_ref/doc/uid/TP40007072-CH7-SW3)  

Screenshots:

{% img /images/blog_images/Screen_shot_2011-09-15_at_PM_12.15.07.png %}  
{% img /images/blog_images/Screen_shot_2011-09-15_at_PM_12.18.57.png %}    

From there, you can find your way! :)