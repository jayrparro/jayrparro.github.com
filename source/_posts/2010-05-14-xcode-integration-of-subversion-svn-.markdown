---
layout: post
title: Xcode Integration of Subversion (SVN)
published: true
comments: true
categories: Xcode, SVN

---
  
Previously, I'm integrating my Xcode project to SVN. I've followed Jeff LaMarche tutorial [here](http://bit.ly/BGPZz) but I've found out it's outdated in the last part since it is based on the previous release of Xcode, I think. The beginning and the middle part is still correct.   
  
So if you're using Xcode 3.2+ like the one I'm using for my project, follow this direction for Jeff's "Not Quite Yet Done" section of his tutorial.   
  
In Xcode 3.2, click the SCM menu →  Configure SCM for this project. A submenu would appear like below:  
{% img http://3.bp.blogspot.com/_DFLTFpvCfQ4/S-zK5sEq-TI/AAAAAAAABLg/7oZuMB7-uzo/s1600/1.png %}  
  
Then, Click on the Configure Roots &amp; SCM, a pop-up menu would appear like the one below:  
{% img http://2.bp.blogspot.com/_DFLTFpvCfQ4/S-zLYDbHyQI/AAAAAAAABLo/N7BtIuN740c/s1600/2.png %}    

Click on that and select the proper repository being configured for this project.   
  
Once you've done that, a new column will appear in your project's Group &amp; Files Pane. If the new column is not visible, right-click the header of the Group &amp; Files Pane then, select SCM.  This new column tells you the status of the file in the repository.    
  
For further reference, please see Apple's [Xcode Source Management Guide](http://bit.ly/98ezRt).  
  
Thanks also to Jeff for a good blog on this stuff! Cheers! :)
