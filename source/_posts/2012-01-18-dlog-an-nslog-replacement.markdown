---
layout: post
title: DLog – an NSLog replacement
published: true
comments: true
categories: iOS

---

It’s more convenient and will add to app performance to log some statements only in Debug mode and avoiding them in release mode.

Plus, we can see in debug console the function & line numbers being logged.  
  
{% codeblock lang:objc %}\#ifdef DEBUG

\#define DLog(fmt, ...) NSLog((@"%s [Line %d] " fmt), __PRETTY_FUNCTION__, __LINE__, ##__VA_ARGS__);  
\#else  
\#define DLog(...)  
\#endif  
\#define ALog(...) NSLog(__VA_ARGS__)
{% endcodeblock %}  
  
  
First add the following to the _Prefix.pch file in your Xcode project:   
In the project’s Debug build configuration, set preprocessor macro **DEBUG=1**  
  
Note:  
DLog – log only in debug mode  
ALog – log for both configurations (debug & release)  
  
More info here:  
<http://kuoi.com/~kamikaze/read.php?topic=Cocoa&id=158>
<http://www.cimgf.com/2009/01/24/dropping-nslog-in-release-builds/>