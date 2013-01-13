---
layout: post
title: Invoke Facetime in-app
published: true
comments: true
categories: iOS

---
Currently, Facetime API is not yet opened as of this writing…  
For the time being, you can invoke Facetime initiating calls through URL:  
{% codeblock lang:objc %}  
NSURL *url = [NSURL URLWithString:@"facetime://+123456789"];
[[UIApplication sharedApplication] openURL:url];
{% endcodeblock %}   
  
* +123456789 - is the Facetime phone number  
  
Hope Facetime API will be open soon... ;)
