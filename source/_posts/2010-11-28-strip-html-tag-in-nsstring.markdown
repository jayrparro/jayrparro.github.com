---
layout: post
title: Strip HTML Tag in NSString
published: true
comments: true
categories: iOS

---
This is a handy method to strip the HTML Tags in NSString object.  
{% codeblock lang:objc %}+(NSString *)flattenHTML:(NSString *)html
{
	NSScanner *thescanner;
	NSString *text = nil;
	thescanner = [NSScanner scannerWithString:html]; 
	
	while ([thescanner isAtEnd] == NO) { 
		//find start of tag
		[thescanner scanUpToString:@"<" intoString:nil];

		//find end of tag
		[thescanner scanUpToString:@">" intoString:&text];

	 	//replace the found tag with a space
		 //(you can filter multi-spaces out later if you wish)
	 	html = [html stringByReplacingOccurrencesOfString:[NSString stringWithFormat:@"%@>", text] withString:@""];
	}

	 // Trimmed return
	 return [html stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceCharacterSet]];
}
{% endcodeblock %}  
  
Thanks to the original [poster.](http://rudis.net/content/2009/01/21/flatten-html-content-ie-strip-tags-cocoaobjective-c)  
I've just change the return string to trim whitespaces.
 
Cheers!