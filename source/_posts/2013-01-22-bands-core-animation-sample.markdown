---
layout: post
title: "Bands - Core Animation Sample"
date: 2013-01-22 00:43
comments: true
categories: [iOS, Core Animation]

---

A simple animation project using Core Animation. It can be use in your App's Splash Screen transition animation.  

#### The Code  
  
To use Core Animation APIs in your project, you need to add the QuartzCore framework. So, in  your Xcode project, go to Target-> Build Phases-> Link Binary With Libraries-> Add QuartzCore.framework
  
{% img /images/blog_images/addQuartzCore.png %}  
  
Then, in ```ViewController.h```:
{% codeblock lang:objc %}
\#import <QuartzCore/QuartzCore.h>  
{% endcodeblock %} 
  
In ```ViewController.m``` file:   
{% codeblock lang:objc %}@interface ViewController () {
    CALayer *bgLayer;
}
{% endcodeblock %}  

```- (void)viewDidLoad``` method:
{% codeblock lang:objc %}bgLayer = [[CALayer alloc] init];    
[bgLayer setBounds:CGRectMake(0.0, 0.0, self.view.bounds.size.width, self.view.bounds.size.height)]; // set size
[bgLayer setPosition:CGPointMake(self.view.frame.size.width/2, self.view.frame.size.height/2)]; // set position
[bgLayer setBackgroundColor:[UIColor blueColor].CGColor];
    
[bgLayer setContents:(id)[UIImage imageNamed:@"image1.jpg"].CGImage];
// [bgLayer setContentsGravity:kCAGravityResizeAspect];
[[self.view layer] addSublayer:bgLayer];    
[self performSelector:@selector(setupBandsAndAnimation) withObject:nil afterDelay:1.0];

{% endcodeblock %}  
  
In the above code, we've setup a ```CALayer```, *bgLayer*, that will hold our contents which is an ```UIImage```. Then, we set its properties and add this layer to the view. After 1 second, we call the method selector ```setupBandsAndAnimation``` to do the splitting of the bands and its animation.  
    
{% codeblock lang:objc %}- (CABasicAnimation *)slideUpAnimation
{  
   //setup animation  
   CABasicAnimation *slideUpAnim = [CABasicAnimation animationWithKeyPath:@"position.y"];  
   [slideUpAnim setToValue:[NSNumber numberWithFloat:-bgLayer.frame.size.height]];  
   [slideUpAnim setDuration:1.0];  
  
   CAMediaTimingFunction *tf = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];  
   [slideUpAnim setTimingFunction:tf];  
  
   return slideUpAnim;
}
{% endcodeblock %}    

{% codeblock lang:objc %}
- (CABasicAnimation *)slideDownAnimation
{  
   //setup animation
   CABasicAnimation *slideDownAnim = [CABasicAnimation animationWithKeyPath:@"position.y"];
   [slideDownAnim setToValue:[NSNumber numberWithFloat:bgLayer.frame.size.height*2]];
   [slideDownAnim setDuration:1.0];  
  
   CAMediaTimingFunction *tf = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];  
   [slideDownAnim setTimingFunction:tf];  
  
   return slideDownAnim;
}
{% endcodeblock %}  
In the above code, we've setup ```CABasicAnimation``` for sliding up & down. We just need to modify the ```position.y``` which is the destination value of the *bgLayer*.
  
{% codeblock lang:objc %}- (void)setupBandsAndAnimation
{
  NSUInteger nNumBands = 20;
  NSMutableArray *bands = [[NSMutableArray alloc] initWithCapacity:nNumBands];  
  CABasicAnimation *slideUpAnim = [self slideUpAnimation];
  CABasicAnimation *slideDownAnim = [self slideDownAnimation];
    
  [bgLayer removeFromSuperlayer];
  [CATransaction begin];
  [CATransaction setCompletionBlock:^(void) {   
   [bands enumerateObjectsUsingBlock:^(id obj, NSUInteger idx, BOOL *stop) {
              [obj setDelegate:nil];
              [obj removeFromSuperlayer];
          }];
    }];  
  
  CGSize layerSize = bgLayer.bounds.size;    
  CGFloat bandWidth = layerSize.width / (CGFloat)nNumBands;
    
  for (int n = 0; n < nNumBands; n++) {  
   CALayer *band = [[CALayer alloc] init];
   band.masksToBounds = YES;  
  
   CGFloat xOffset = 1.f / (CGFloat)nNumBands;
   band.bounds = CGRectMake(0.f, 0.f, bandWidth, layerSize.height);
   band.contents = bgLayer.contents;
   band.contentsGravity = kCAGravityCenter;
   band.contentsRect = CGRectMake(xOffset * n , 0.f, xOffset, 1.f);
        
   CGPoint bandOrigin = bgLayer.frame.origin;
   bandOrigin.x = bandOrigin.x + (bandWidth * n);
   [band setValue:[NSValue valueWithCGPoint:bandOrigin] forKeyPath:@"frame.origin"];
        
   [self.view.layer addSublayer:band];
        
   [band addAnimation:(n % 2) ? slideUpAnim : slideDownAnim forKey:nil];
   [bands addObject:band];
  }
    
  [CATransaction commit];
}
{% endcodeblock %}  
  
In the above code, we specify how many bands that it need to split up (*nNumBands*). Then, we've called the 2 ```CABasicAnimation``` we setup earlier. We iterate on the number of small bands that's been split up, created another ```CaLayer``` object and setting up its properties and apply the animation. After the animation is finished, we remove the layer, ```[obj removeFromSuperLayer]```.


####Sample Output
<iframe width="420" height="315" src="http://www.youtube.com/embed/X1dexZH2RVA?rel=0" frameborder="0" allowfullscreen></iframe>  
  

<br/>
You can get the code [here](https://github.com/jayrparro/Labs/tree/master/Bands).  
  
Happy Coding! :)
<br/>  
PS: Further reading:  
[Core Animation Programming Guide](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CoreAnimation_guide/Introduction/Introduction.html)