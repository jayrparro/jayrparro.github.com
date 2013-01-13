---
layout: post
title: DLog – an NSLog replacement
published: true
comments: true
categories: iOS

---

<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;"><span style="color: #737373; font-family: Helvetica Neue, Helvetica, Hiragino Sans GB, Arial, sans-serif; line-height: 18px;">
<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;">It&rsquo;s more convenient and will add to app performance to log some statements only in Debug mode and avoiding them in release mode.</p>
<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;">Plus, we can see in debug console the function &amp; line numbers being logged.</p>
<div class="CodeRay">
  <div class="code"><pre>#ifdef DEBUG

#define DLog(fmt, ...) NSLog((@&quot;%s [Line %d] &quot; fmt), __PRETTY_FUNCTION__, __LINE__, ##__VA_ARGS__);

#else

#define DLog(...)

#endif

#define ALog(...) NSLog(__VA_ARGS__)</pre></div>
</div>

<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;">First add the following to the&nbsp;_Prefix.pch file in your Xcode project:</p>
<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;">In the project&rsquo;s Debug build configuration, set preprocessor macro DEBUG=1</p>
<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;">&nbsp;</p>
<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;"><strong>Note:</strong></p>
<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;">DLog &ndash; log only in debug mode</p>
<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;">ALog &ndash; log for both configurations (debug &amp; release)</p>
<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;">&nbsp;</p>
<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;"><strong>More info here:</strong></p>
<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;"><a href="http://kuoi.com/~kamikaze/read.php?topic=Cocoa&amp;id=158" style="color: #0069d6;">http://kuoi.com/~kamikaze/read.php?topic=Cocoa&amp;id=158</a></p>
<p style="margin-top: 0px; margin-right: 0px; margin-bottom: 9px; margin-left: 0px; padding: 0px;"><a href="http://www.cimgf.com/2009/01/24/dropping-nslog-in-release-builds/" style="color: #0069d6;">http://www.cimgf.com/2009/01/24/dropping-nslog-in-release-builds/</a></p>
</span></p>
<p>&nbsp;</p>
