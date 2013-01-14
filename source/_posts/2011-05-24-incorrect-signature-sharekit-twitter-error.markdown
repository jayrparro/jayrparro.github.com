---
layout: post
title: Incorrect Signature - ShareKit+Twitter error
published: true
comments: true
categories: [iOS, Twitter]

---
  
I'm using ShareKit  v0.2.1 in our app to integrate FB and Twitter status updates. The FB service is working fine. However, in the Twitter service I encountered this error:  
{% codeblock lang:objc %}Twitter Send Status Error:{ 
	"request":"\/1\/statuses\/update.json",
    "error":"Incorrect signature"
}
{% endcodeblock %}  
  
I'm just concerned of sharing the URL to be posted in the user's wall.   
Reported the problem/bug [here](http://bit.ly/kPSHU1).

I found out that the Twitter oAuth has been updated and breaks in this latest release of ShareKit.

Excerpts from Twitter oAuth docs (<https://dev.twitter.com/doc/get/oauth/authorize>):   
>Please use HTTPS for this method, and all other OAuth token negotiation steps.  
  
To fix this issue, we need to change SHKTwitter.m in lines 54-56   
from **https://twitter.com/** to **https://api.twitter.com/**     
{% codeblock lang:objc %}self.authorizeURL = [NSURL URLWithString:@"https://api.twitter.com/oauth/authorize"];
self.requestURL = [NSURL URLWithString:@"https://api.twitter.com/oauth/request_token"];
self.accessURL = [NSURL URLWithString:@"https://api.twitter.com/oauth/access_token"];
{% endcodeblock %}  
  
Then, in sendStatus method update the URL link:  
{% codeblock lang:objc %}OAMutableURLRequest *oRequest = [[OAMutableURLRequest alloc] initWithURL:[NSURL URLWithString:@"https://api.twitter.com/1/statuses/update.json"] consumer:consumer token:accessToken realm:nil signatureProvider:nil];
{% endcodeblock %}  
  
This should be fix in the next version of ShareKit. :)
