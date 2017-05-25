---
layout: post
title:  "MapView with path between two points for iPhone using Xcode 4.6"
redirect_from:
   - /mapview-path-points-iphone-xcode-4-6
date:   2013-06-28 16:13:18 +0100
categories: best domain registrar
description: .
---

I recently developed an iPhone app for an event in Sweden called SFSFUM which required a map which would show the user directions from their current location to several of the different locations of the event. Since there was no available tools in xcode to do this I had to create some custom code and help from contributions on github to accomplish the tasks and draw the path. I found the sample application MapWithRoutes by Kadir Pekel which can be found on github to be very helpful: [https://github.com/kadirpekel/MapWithRoute](https://github.com/kadirpekel/MapWithRoutes "MapWithRoutes") I also made use of RegexKitLite: [http://regexkit.sourceforge.net/](http://regexkit.sourceforge.net/ "RegexKitLite") I start by creating a MapViewController view which includes a MapView like so (image removed) Here is the code for my header file

```
<pre lang="php">//
//  MapViewController.h
//  sfsfum
//
//  Created by MarkusTenghamn on 3/9/13.
//  Copyright (c) 2013 MarkusTenghamn. All rights reserved.
//

#import <UIKit/UIKit.h>
#import <CoreLocation/CoreLocation.h>
#import <MapKit/MapKit.h>
#import <UIKit/UIKit.h>
#import "Classes/MapView.h"
#import "Classes/Place.h"

@interface MapViewController : UIViewController

@property (strong, nonatomic) IBOutlet MapView *mapView;

@property (nonatomic, strong) id Delegate;
@property (nonatomic, strong) NSString *address;
@property (nonatomic, strong) NSString *objecttitle;

- (IBAction)BackBtnPress:(id)sender;

@end
``` I then use the google API along with the address of the location in order to get the coordinates. If you already know the coordinates then you could skip this step and save some time when loading the map. I preferred to do it this way for this particular app. Here is the code for the MapViewController. ```
<pre lang="php">//
//  MapViewController.m
//  sfsfum
//
//  Created by MarkusTenghamn on 3/9/13.
//  Copyright (c) 2013 MarkusTenghamn. All rights reserved.
//

#import "MapViewController.h"
#import <CoreLocation/CoreLocation.h>
#import <MapKit/MapKit.h>
#import <MapKit/MKAnnotation.h>
#import "AddressAnnotation.h"

@interface MapViewController ()

@end

@implementation MapViewController

@synthesize Delegate;
@synthesize address = _address;
@synthesize mapView;

- (id)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil
{
    self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];
    if (self) {
        // Custom initialization
    }
    return self;
}

- (void)viewWillAppear:(BOOL)animated {

}

- (void)viewDidLoad
{
    [super viewDidLoad];
    CLLocationCoordinate2D coord = [self geoCodeUsingAddress:self.address];

    mapView = [[MapView alloc] initWithFrame:
						 CGRectMake(0, 60, self.view.frame.size.width, self.view.frame.size.height-60)];

	[self.view addSubview:mapView];

    CLLocationManager *locationManager = [[CLLocationManager alloc]init];
	Place* to = [[Place alloc] init];
	to.name = self.title;
	to.description = self.address;
	to.latitude = (double)coord.latitude;
	to.longitude = (double)coord.longitude;

	Place* from = [[Place alloc] init];
	from.name = @"Start";
	from.description = @"";

    from.latitude = (double)locationManager.location.coordinate.latitude;
	from.longitude = (double)locationManager.location.coordinate.longitude;

	if (!(from.longitude == 0 && from.latitude == 0) && !(((fabs(to.latitude) - fabs(from.latitude)) > 5) || ((fabs(to.longitude) - fabs(from.longitude)) > 5))) {
        [mapView showRouteFrom:from to:to];
    } else {
        from.longitude = to.longitude-0.01;
        from.latitude = to.latitude-0.01;
        [mapView showRouteFrom:from to:to];
    }
}

- (void)didReceiveMemoryWarning
{
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

- (CLLocationCoordinate2D) geoCodeUsingAddress:(NSString *)address
{
    NSString *esc_addr =  [address stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
    NSString *req = [NSString stringWithFormat:@"http://maps.google.com/maps/api/geocode/json?sensor=true&address=%@", esc_addr];
    CLLocationCoordinate2D center;

    NSString *result = [NSString stringWithContentsOfURL:[NSURL URLWithString:req] encoding:NSUTF8StringEncoding error:NULL];
    if (result)
    {
        NSLog(@"LOC RESULT: %@", result);
        NSError *e;
        NSDictionary *resultDict = [NSJSONSerialization JSONObjectWithData: [result dataUsingEncoding:NSUTF8StringEncoding]
                                                                   options: NSJSONReadingMutableContainers
                                                                     error: &e];
        NSArray *resultsArray = [resultDict objectForKey:@"results"];

        if(resultsArray.count > 0)
        {
            resultDict = [[resultsArray objectAtIndex:0] objectForKey:@"geometry"];
            resultDict = [resultDict objectForKey:@"location"];
            center.latitude = [[resultDict objectForKey:@"lat"] floatValue];
            center.longitude = [[resultDict objectForKey:@"lng"] floatValue];
            NSLog(@"Long: %f Lat: %f", center.longitude, center.latitude);
        }
        else
        {
            UIAlertView *alert = [[UIAlertView alloc] initWithTitle:@"Inget att visa" message:@"Det gick inte att visa den plats du valt. Försök igen." delegate:self cancelButtonTitle:@"Ok" otherButtonTitles: nil];
            [alert show];
        }
    }
    return center;
}

- (IBAction)BackBtnPress:(id)sender {
    [self dismissViewControllerAnimated:YES completion:nil];
}
@end
``` I also use an AddressAnnotation class to show a pin. ```
<pre lang="php">//
//  AddressAnnotation.h
//  sfsfum
//
//  Created by MarkusTenghamn on 3/9/13.
//  Copyright (c) 2013 MarkusTenghamn. All rights reserved.
//

#import <Foundation/Foundation.h>
#import <MapKit/MapKit.h>

@interface AddressAnnotation : NSObject {
    CLLocationCoordinate2D coordinate;
}

@property (nonatomic, strong) NSString *mTitle;
@property (nonatomic, strong) NSString *mSubTitle;

-(id)initWithCoordinate:(CLLocationCoordinate2D)c;
@end
``` And ```
<pre lang="php">//
//  AddressAnnotation.m
//  sfsfum
//
//  Created by MarkusTenghamn on 3/9/13.
//  Copyright (c) 2013 MarkusTenghamn. All rights reserved.
//

#import "AddressAnnotation.h"

@implementation AddressAnnotation

@synthesize coordinate;
@synthesize mTitle;
@synthesize mSubTitle;

- (NSString *) title {
    if (mTitle) {
        return mTitle;
    }
    return @"Title";
}

- (NSString *) subtitle {
    if (mSubTitle) {
        return mSubTitle;
    }
    return @"Subtitle";
}

-(id)initWithCoordinate:(CLLocationCoordinate2D) c{
    coordinate=c;
    NSLog(@"%f,%f",c.latitude,c.longitude);
    return self;
}

@end
``` I can then call the MapViewController like so ```
<pre lang="php">MapViewController *map = [[MapViewController alloc] initWithNibName:@"MapViewController" bundle:nil];
    map.Delegate = self;
    map.modalTransitionStyle = UIModalTransitionStyleFlipHorizontal;
    map.address = [[self.menus objectAtIndex:indexPath.row] objectForKey:@"ADD"];
    map.title = [[self.menus objectAtIndex:indexPath.row] objectForKey:@"TITLE"];
    [self presentViewController:map animated:YES completion:nil];
``` That's it, I hope the sample code helps, let me know if you have any questions or comments.