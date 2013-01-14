---
layout: post
title: ! 'MyLocation: A CoreLocation Tutorial'
published: true
comments: true
categories: [iOS, Tutorials]

---
  
This tutorial provide the basics for using the CoreLocation API in an iPhone app. The goal of this tutorial is to build a simple application, **MyLocation**. This application will tell you your current location and update itself when you move.     

####**Core Location Fundamentals:**
  
The Core Location API is event-based. The phone gets an update when Core Location is running and new location is available or changes. The main class that do the work is ***CLLocationManager***, usually referred to as the Location Manager. To use Core Location, we need to create and instantiate the ***CLLocationManager*** class, like this:   
  
{% codeblock lang:objc %}
CLLocationManager *locationManager = [CLLocationManager alloc] init];
{% endcodeblock %}  
  
Then, the Location Manager will call the  delegate methods when new location is available. This delegate method must conform to ***CLLocationManagerDelegate*** protocol.  
  
####***CLLocationManager*** Properties and methods:  


- Desired Accuracy

	After setting the delegate, we must set the requested accuracy of the location data. Assign a value to this property appropriated only for the application usage and scenario. The more accuracy you demand, the more juice you're likely to use. The value is in meters. Example of setting the delegate and requesting a specific level of accuracy:  
{% codeblock lang:objc %}
locationManager.delegate = self;   
locationManager.desiredAccuracy = kCLLocationAccuracyBest;  
{% endcodeblock %}
    
- Distance Filter

	The minimum distance (measured in meters) a device must move laterally before an update event is generated. The default value of this property is **kCLDistanceFilterNone**. Specifying a distance filter of 1000 tells the Location Manager not to notify its delegate until the iPhone has moved at least 1,000 meters from its previously reported position.   
Here’s an example:    
{% codeblock lang:objc %}
locationManager.distanceFilter = 1000.0f;
{% endcodeblock %}

  
####Starting and Stopping updates:   
When you start polling location, you invoke this 
line to start the Location Manager  
{% codeblock lang:objc %}
[locationManager startUpdatingLocation];
{% endcodeblock %}    

It will eventually call its delegate method when it has determined the current location and senses any changes of the location.  
When you already determined the information needed, invoke the delegate to stop the Location Manager by this line:  
{% codeblock lang:objc %}
[locationManager stopUpdatingLocation];
{% endcodeblock %}      
  
####Getting the latitude and longitude:
  
The Location Manager passes the location data using instances of the *CLLocation* class. The latitude and longitude are stored in a property called *coordinate*.   
To get the latitude and longitude in degrees, do this:  
{% codeblock lang:objc %}
CLLocationDegrees latitude = theLocation.coordinate.latitude  
CLLocationDegrees longitude = theLocation.coordinate.longitude;
{% endcodeblock %}        
  
####Location Manager Delegate
  
The Location Manager delegate must conform to the **CLLocationManagerDelegate** protocol. The protocol have two methods:    
{% codeblock lang:objc %}
- (void)locationManager:(CLLocationManager *)manager didUpdateToLocation:(CLLocation *)newLocation fromLocation:(CLLocation *)oldLocation  
{% endcodeblock %} Tells the delegate that a new location value is available.  
  
  
{% codeblock lang:objc %}
-(void)locationManager:(CLLocationManager *)manager didFailWithError:(NSError *)error  
{% endcodeblock %}  
Tells the delegate that the location manager was unable to retrieve a location value.  
  
####Creating the Project:  
1. Create a new project using File-&gt; New Project… from Xcode’s menu
2. Select View-Based Application from the iPhone OS -&gt; Application section, click Choose…
3. Name the project as MyLocation and click Save

  
####Modifying the Controller Class MyLocationViewController.m/.h

1. We'll start by adding the CoreLocation framework to the Project
2. In the Groups &amp; Files panel of the project, expand the MyLocation project branch
3. Control-click or right-click the Frameworks folder
4. Choose Add -&gt; Existing Frameworks…
5. Expand the Frameworks folder
6. Choose *CoreLocation.framework* and click Add
      
Next, click MyLocationViewController.h and write the following code:  
{% codeblock lang:objc %}
@interface MyLocationViewController : UIViewController <CLLocationManagerDelegate>
{
	CLLocationManager *m_locationManager;
	CLLocation *m_locationStartingPoint;

	//UI Outlets
	UILabel *m_latitudeLabel;
	UILabel *m_longitudeLabel;
}

@property(nonatomic,retain) CLLocationManager *m_locationManager;
@property(nonatomic,retain) CLLocation *m_locationStartingPoint;
@property(nonatomic,retain) IBOutlet UILabel *m_latitudeLabel;
@property(nonatomic,retain) IBOutlet UILabel *m_longitudeLabel;

@end
{% endcodeblock %}   
  
Explanation of .h file:
First, we import the Core Location header file.  
The class must conform to the CLLocationManagerDelegate protocol.  
We declare two UILabels for output in the view. IBOutlet is a macro that tells the compiler that it needs to wire up this variable to a corresponding UILabel element added in Interface Builder.   
  
####Design the view of the application:  
Click the MyLocationViewController.xib to open up the Interface Builder.

1. In the Groups &amp; Files panel of the project, expand the MyLocation project branch
2. Control-click or right-click the Resources folder
3. Double-click the MyLocationViewController.xib file  
  
Make sure that the Library, Inspector and View windows are open/visible.   
If they are not:  

1. Show the Library window using Tools &gt; Library from the menu
2. Show the Inspector window using Tools &gt; Inspector from the menu 
3. Show the View by clicking the View icon on the HelloThereViewController.xib window 

Adding the label:  
Locate the Label component in the Library window and drag it onto the view.   
UI output looks like this:   
<p/>
{% img http://2.bp.blogspot.com/_DFLTFpvCfQ4/S_AhSZRCyBI/AAAAAAAABLw/iwedmhfYzE0/s1600/Screen+shot+2010-05-16+at+11.37.34+PM.png %}
  
<p />Connect the Interface Builder label the code’s m_latitudeLabel, m_longitudeLabel. While still in Interface Builder:<br />Control-click on File’s Owner icon in the MyLocationViewController.xib window<p /><ol>
<li>In the resulting pop-up menu, click-and-hold (i.e., don’t unclick) on the right-justified circle in the m_latitudeLabel row of the Outlets section</li>
<li>Drag the mouse to the Label in the View. A blue line will connect the two.</li>
<li>Do this for the remaining label</li>
</ol>  
  
Confirm that the two have been connected. The pop-up menu should look like the image below:  
<p/>    
{% img http://4.bp.blogspot.com/_DFLTFpvCfQ4/S_AhtXlAwNI/AAAAAAAABL4/6r2Fs_53zLg/s1600/Screen+shot+2010-05-16+at+11.38.24+PM.png %}  
  
<p/>  
If everything looks right, save the changes and close Interface Builder.
Switch back to Xcode, implement the MyLocationViewController.m
  
{% codeblock lang:objc %}
\#import <UIKit/UIKit.h>
\#mport <CoreLocation/CoreLocation.h>

@implementation MyLocationViewController
	@synthesize m_locationManager;
	@synthesize m_latitudeLabel;
	@synthesize m_longitudeLabel;
	
- (void)viewDidLoad
{
	[super viewDidLoad]; 
	self.m_locationManager = [[CLLocationManager alloc] init];
	m_locationManager.delegate = self;
	m_locationManager.desiredAccuracy = kCLLocationAccuracyBest;
	
	[m_locationManager startUpdatingLocation];
}

- (void)viewDidUnload 
{ 
	self.m_latitudeLabel = nil;
	self.m_longitudeLabel = nil; 
	self.m_locationManager = nil;
	[super viewDidUnload];
}
	
- (void)dealloc  {
	[m_latitudeLabel release];
	[m_longitudeLabel release];
	[m_locationManager release];
	[m_locationStartingPoint release];
	[super dealloc];
}

- (void)locationManager:(CLLocationManager *)manager 
didUpdateToLocation:(CLLocation *)newLocation 
fromLocation:(CLLocation *)oldLocation
{   
	NSString *latitudeString = [[NSString alloc] initWithFormat:@"%g°", newLocation.coordinate.latitude];
	m_latitudeLabel.text = latitudeString;
	[latitudeString release];
	
	NSString *longitudeString = [[NSString alloc] initWithFormat:@"%g°", newLocation.coordinate.longitude];
	m_longitudeLabel.text = longitudeString;
	[longitudeString release];	
}

- (void)locationManager:(CLLocationManager *)manager 
didFailWithError:(NSError *)error
{
	NSString *errorTypeString = (error.code == kCLErrorDenied) ? 
		@"Access Denied" : @"Unknown Error";
		
	UIAlertView *alert = [[UIAlertView alloc] 
		initWithTitle:@"Error getting location" 
		message:errorTypeString 
		delegate:nil 
		cancelButtonTitle:@"OK" 
		otherButtonTitles:nil];
	
	[alert show];
	[alert release];	
} 
@end

{% endcodeblock %}  
  
<p />Build and go! ;)