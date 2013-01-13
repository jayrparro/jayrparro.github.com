---
layout: post
title: Adding drop shadow effect in a View
published: true
comments: true
categories: iOS

---
A simple implementation of adding a drop shadow effect in a view instead of using “extra” shadow images.

.h file:
{% codeblock lang:objc %}
\#import <QuartzCore/QuartzCore.h>;
{% endcodeblock %}  
  
.m file:  
{% codeblock lang:objc %}
	// add drop shadow
	self.tempView.layer.masksToBounds = NO;
	self.tempView.layer.shadowOffset = CGSizeMake(-10, 10);
	self.tempView.layer.cornerRadius = 10; // round corner
	self.tempView.layer.shadowRadius = 5;
	self.tempView.layer.shadowOpacity = 0.5;
	self.tempView.layer.shadowPath = [UIBezierPath bezierPathWithRect:self.tempView.bounds].CGPath; // drawing optimization
{% endcodeblock %}

Initial View:  
{% img /images/blog_images/initView.png.scaled500.png %}  
  
Output View:  
{% img /images/blog_images/outView.png.scaled500.png %}  

You can download the code [here](https://github.com/jayrparro/Labs/tree/master/DropShadow).   
  
Happy Coding! :)
