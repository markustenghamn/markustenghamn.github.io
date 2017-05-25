---
layout: post
title:  "MapView with path between two points for iPhone using Xcode 4.6"
redirect_from:
   - /mapview-path-points-iphone-xcode-4-6
date:   2013-06-28 16:13:18 +0100
categories: best domain registrar
description: I recently developed an iPhone app for an event in Sweden called SFSFUM which required a map which would show the user directions from their current location to several of the different locations of t...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post so it may be hard to read but I have left it here as a reference.**

I recently developed an iPhone app for an event in Sweden called SFSFUM which required a map which would show the user directions from their current location to several of the different locations of the event. Since there was no available tools in xcode to do this I had to create some custom code and help from contributions on github to accomplish the tasks and draw the path.  
  
 I found the sample application MapWithRoutes by Kadir Pekel which can be found on github to be very helpful: [https://github.com/kadirpekel/MapWithRoute](https://github.com/kadirpekel/MapWithRoutes "MapWithRoutes")  
  
 I also made use of RegexKitLite: [http://regexkit.sourceforge.net/](http://regexkit.sourceforge.net/ "RegexKitLite")  
  
 I start by creating a MapViewController view which includes a MapView like so  
  
 (image removed)   
   
  
 Here is the code for my header file

```
<pre lang="php">//<br></br>
//  MapViewController.h<br></br>
//  sfsfum<br></br>
//<br></br>
//  Created by MarkusTenghamn on 3/9/13.<br></br>
//  Copyright (c) 2013 MarkusTenghamn. All rights reserved.<br></br>
//<br></br><br></br>
#import <UIKit/UIKit.h><br></br>
#import <CoreLocation/CoreLocation.h><br></br>
#import <MapKit/MapKit.h><br></br>
#import <UIKit/UIKit.h><br></br>
#import "Classes/MapView.h"<br></br>
#import "Classes/Place.h"<br></br><br></br>
@interface MapViewController : UIViewController<br></br><br></br>
@property (strong, nonatomic) IBOutlet MapView *mapView;<br></br><br></br>
@property (nonatomic, strong) id Delegate;<br></br>
@property (nonatomic, strong) NSString *address;<br></br>
@property (nonatomic, strong) NSString *objecttitle;<br></br><br></br>
- (IBAction)BackBtnPress:(id)sender;<br></br><br></br>
@end
```  
   
  
 I then use the google API along with the address of the location in order to get the coordinates. If you already know the coordinates then you could skip this step and save some time when loading the map. I preferred to do it this way for this particular app. Here is the code for the MapViewController.  
```
<pre lang="php">//<br></br>
//  MapViewController.m<br></br>
//  sfsfum<br></br>
//<br></br>
//  Created by MarkusTenghamn on 3/9/13.<br></br>
//  Copyright (c) 2013 MarkusTenghamn. All rights reserved.<br></br>
//<br></br><br></br>
#import "MapViewController.h"<br></br>
#import <CoreLocation/CoreLocation.h><br></br>
#import <MapKit/MapKit.h><br></br>
#import <MapKit/MKAnnotation.h><br></br>
#import "AddressAnnotation.h"<br></br><br></br>
@interface MapViewController ()<br></br><br></br>
@end<br></br><br></br>
@implementation MapViewController<br></br><br></br>
@synthesize Delegate;<br></br>
@synthesize address = _address;<br></br>
@synthesize mapView;<br></br><br></br>
- (id)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil<br></br>
{<br></br>
    self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];<br></br>
    if (self) {<br></br>
        // Custom initialization<br></br>
    }<br></br>
    return self;<br></br>
}<br></br><br></br>
- (void)viewWillAppear:(BOOL)animated {<br></br><br></br>
}<br></br><br></br>
- (void)viewDidLoad<br></br>
{<br></br>
    [super viewDidLoad];<br></br>
    CLLocationCoordinate2D coord = [self geoCodeUsingAddress:self.address];<br></br><br></br>
    mapView = [[MapView alloc] initWithFrame:<br></br>
						 CGRectMake(0, 60, self.view.frame.size.width, self.view.frame.size.height-60)];<br></br><br></br>
	[self.view addSubview:mapView];<br></br><br></br>
    CLLocationManager *locationManager = [[CLLocationManager alloc]init];<br></br>
	Place* to = [[Place alloc] init];<br></br>
	to.name = self.title;<br></br>
	to.description = self.address;<br></br>
	to.latitude = (double)coord.latitude;<br></br>
	to.longitude = (double)coord.longitude;<br></br><br></br>
	Place* from = [[Place alloc] init];<br></br>
	from.name = @"Start";<br></br>
	from.description = @"";<br></br><br></br>
    from.latitude = (double)locationManager.location.coordinate.latitude;<br></br>
	from.longitude = (double)locationManager.location.coordinate.longitude;<br></br><br></br>
	if (!(from.longitude == 0 && from.latitude == 0) && !(((fabs(to.latitude) - fabs(from.latitude)) > 5) || ((fabs(to.longitude) - fabs(from.longitude)) > 5))) {<br></br>
        [mapView showRouteFrom:from to:to];<br></br>
    } else {<br></br>
        from.longitude = to.longitude-0.01;<br></br>
        from.latitude = to.latitude-0.01;<br></br>
        [mapView showRouteFrom:from to:to];<br></br>
    }<br></br>
}<br></br><br></br>
- (void)didReceiveMemoryWarning<br></br>
{<br></br>
    [super didReceiveMemoryWarning];<br></br>
    // Dispose of any resources that can be recreated.<br></br>
}<br></br><br></br>
- (CLLocationCoordinate2D) geoCodeUsingAddress:(NSString *)address<br></br>
{<br></br>
    NSString *esc_addr =  [address stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];<br></br>
    NSString *req = [NSString stringWithFormat:@"http://maps.google.com/maps/api/geocode/json?sensor=true&address=%@", esc_addr];<br></br>
    CLLocationCoordinate2D center;<br></br><br></br>
    NSString *result = [NSString stringWithContentsOfURL:[NSURL URLWithString:req] encoding:NSUTF8StringEncoding error:NULL];<br></br>
    if (result)<br></br>
    {<br></br>
        NSLog(@"LOC RESULT: %@", result);<br></br>
        NSError *e;<br></br>
        NSDictionary *resultDict = [NSJSONSerialization JSONObjectWithData: [result dataUsingEncoding:NSUTF8StringEncoding]<br></br>
                                                                   options: NSJSONReadingMutableContainers<br></br>
                                                                     error: &e];<br></br>
        NSArray *resultsArray = [resultDict objectForKey:@"results"];<br></br><br></br>
        if(resultsArray.count > 0)<br></br>
        {<br></br>
            resultDict = [[resultsArray objectAtIndex:0] objectForKey:@"geometry"];<br></br>
            resultDict = [resultDict objectForKey:@"location"];<br></br>
            center.latitude = [[resultDict objectForKey:@"lat"] floatValue];<br></br>
            center.longitude = [[resultDict objectForKey:@"lng"] floatValue];<br></br>
            NSLog(@"Long: %f Lat: %f", center.longitude, center.latitude);<br></br>
        }<br></br>
        else<br></br>
        {<br></br>
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Inget att visa" message:@"Det gick inte att visa den plats du valt. Försök igen." delegate:self cancelButtonTitle:@"Ok" otherButtonTitles: nil];<br></br>
            [alert show];<br></br>
        }<br></br>
    }<br></br>
    return center;<br></br>
}<br></br><br></br>
- (IBAction)BackBtnPress:(id)sender {<br></br>
    [self dismissViewControllerAnimated:YES completion:nil];<br></br>
}<br></br>
@end
```  
   
  
 I also use an AddressAnnotation class to show a pin.  
```
<pre lang="php">//<br></br>
//  AddressAnnotation.h<br></br>
//  sfsfum<br></br>
//<br></br>
//  Created by MarkusTenghamn on 3/9/13.<br></br>
//  Copyright (c) 2013 MarkusTenghamn. All rights reserved.<br></br>
//<br></br><br></br>
#import <Foundation/Foundation.h><br></br>
#import <MapKit/MapKit.h><br></br><br></br>
@interface AddressAnnotation : NSObject {<br></br>
    CLLocationCoordinate2D coordinate;<br></br>
}<br></br><br></br>
@property (nonatomic, strong) NSString *mTitle;<br></br>
@property (nonatomic, strong) NSString *mSubTitle;<br></br><br></br>
-(id)initWithCoordinate:(CLLocationCoordinate2D)c;<br></br>
@end
```  
   
  
 And  
```
<pre lang="php">//<br></br>
//  AddressAnnotation.m<br></br>
//  sfsfum<br></br>
//<br></br>
//  Created by MarkusTenghamn on 3/9/13.<br></br>
//  Copyright (c) 2013 MarkusTenghamn. All rights reserved.<br></br>
//<br></br><br></br>
#import "AddressAnnotation.h"<br></br><br></br>
@implementation AddressAnnotation<br></br><br></br>
@synthesize coordinate;<br></br>
@synthesize mTitle;<br></br>
@synthesize mSubTitle;<br></br><br></br>
- (NSString *) title {<br></br>
    if (mTitle) {<br></br>
        return mTitle;<br></br>
    }<br></br>
    return @"Title";<br></br>
}<br></br><br></br>
- (NSString *) subtitle {<br></br>
    if (mSubTitle) {<br></br>
        return mSubTitle;<br></br>
    }<br></br>
    return @"Subtitle";<br></br>
}<br></br><br></br>
-(id)initWithCoordinate:(CLLocationCoordinate2D) c{<br></br>
    coordinate=c;<br></br>
    NSLog(@"%f,%f",c.latitude,c.longitude);<br></br>
    return self;<br></br>
}<br></br><br></br>
@end
```  
   
  
 I can then call the MapViewController like so  
```
<pre lang="php">MapViewController *map = [[MapViewController alloc] initWithNibName:@"MapViewController" bundle:nil];<br></br>
    map.Delegate = self;<br></br>
    map.modalTransitionStyle = UIModalTransitionStyleFlipHorizontal;<br></br>
    map.address = [[self.menus objectAtIndex:indexPath.row] objectForKey:@"ADD"];<br></br>
    map.title = [[self.menus objectAtIndex:indexPath.row] objectForKey:@"TITLE"];<br></br>
    [self presentViewController:map animated:YES completion:nil];
```  
   
  
 That's it, I hope the sample code helps, let me know if you have any questions or comments.