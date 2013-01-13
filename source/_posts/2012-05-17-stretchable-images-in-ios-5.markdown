---
layout: post
title: Stretchable Images in iOS 5
published: true
comments: true
categories: iOS

---

In iOS 5, working with UIImage *stretchableImageWithLeftCapWidth:topCapHeight:* is deprecated; in which I've used it heavily to reduced app size, dynamic variable-width buttons, and less image files added in the project.
  
**stretchableImageWithLeftCapWidth:topCapHeight:  **

{% codeblock lang:objc %}
// Creates and returns a new image object with the specified cap values   
// (Deprecated in iOS 5.0. Deprecated.   
// Use the resizableImageWithCapInsets: instead,   
// specifying cap insets such that the interior is a 1x1 area.)

-(UIImage *)stretchableImageWithLeftCapWidth:(NSInteger)leftCapWidth topCapHeight:(NSInteger)topCapHeight
{% endcodeblock %}


So, looking forward to support stretchable images in iOS 5, we'll have to use this new method
  
**resizableImageWithCapInsets:**  
{% codeblock lang:objc %}  
// Creates and returns a new image object with the specified cap insets.  
-(UIImage *)resizableImageWithCapInsets:(UIEdgeInsets)capInsets
{% endcodeblock %}  

**Parameters**   
capInsets - The values to use for the cap insets.   
  
Return Value - A new image object with the specified cap insets.

**Discussion** -   
You use this method to add cap insets to an image or to change the existing cap insets of an image. In both cases, you get back a new image and the original image remains untouched.

During scaling or resizing of the image, areas covered by a cap are not scaled or resized. Instead, the pixel area not covered by the cap in each direction is tiled, left-to-right and top-to-bottom, to resize the image. This technique is often used to create variable-width buttons, which retain the same rounded corners but whose center region grows or shrinks as needed. For best performance, use a tiled area that is a 1x1 pixel area in size.

>
As noted in the documentation, cap insets of the image are pixel area that are not affected when the image is scaled or resized dynamically. The pixels beyond the cap area are being tiled.


In this example, I&rsquo;ve use this image with dimension of 12 (width) x 43 (height) pixels. You need to check in Photoshop the actual cap pixels area (or should ask your designer). The sample image I&rsquo;ve use has rounded corner in the top, which is specifically, 6 pixels width, and that&rsquo;s the cap area that I don&rsquo;t want to be scaled/resized.

{% img /images/blog_images/blue-header-frame.jpg.scaled500.jpg %}

In pre-iOS 5, we&rsquo;ve use the stretchableImageWithLeftCapWidth:topCapHeight:  
{% codeblock lang:objc %}// note- stretchableImageWithLeftCapWidth deprecated in iOS 5
UIImage* image1 = [[UIImage imageNamed: FILE_IMG_BLUE_HEADER] stretchableImageWithLeftCapWidth:6.0 topCapHeight:0.0];

UIImageView* imageView1 = [[UIImageView alloc] initWithImage:image1];
imageView1.frame = CGRectMake(20, 115, 100.0, image1.size.height);
[self.view addSubview:imageView1];
{% endcodeblock %}
  
The value parameter stretchableImageWithLeftCapWidth:6.0 is the left cap width pixel (6 pixels width as noted in our sample image).  

In iOS 5+ onwards, we use resizableImageWithCapInsets:  
{% codeblock lang:objc %}
// note- resizableImageWithCapInsets use in iOS 5+
self.imageView3.image = [[UIImage imageNamed: FILE_IMG_BLUE_HEADER] resizableImageWithCapInsets:UIEdgeInsetsMake(0.0, 6.0, 0.0, 6.0)];
{% endcodeblock %}  

I've put a value for UIEdgeInsetsMake for both *left* and *right* which is 6-pixels cap width. Then, UIEdgeInsetsMake returns a UIEdgeInsets data struct-
{% codeblock lang:objc %}    
typedef struct {
    CGFloat top, left, bottom, right;
} UIEdgeInsets;
{% endcodeblock %}  
  
Output:    

1. An imageView in which the image is just stretch and&nbsp;<em>no</em>cap/tiled being applied. It looks ugly.
2. An imageView in which the image is stretch and cap/tiled pixels are applied. The imageView is added in the view programmatically.
3. The image is stretch and cap/tiled pixels are applied. The imageView is added via an outlet.
  
Screenshot:  
{% img /images/blog_images/Screen_Shot_2012-05-16_at_6.13.08_PM.png.scaled500.png %}    

You can download the sample code [here](https://github.com/jayrparro/Labs/tree/master/StretchableImage).   
  
Happy Coding! :)
