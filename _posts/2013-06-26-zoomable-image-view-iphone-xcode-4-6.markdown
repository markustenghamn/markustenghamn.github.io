---
layout: post
title:  "Zoomable image view for iPhone in Xcode 4.6"
redirect_from:
   - /zoomable-image-view-iphone-xcode-4-6
date:   2013-06-26 23:24:49 +0100
categories: best domain registrar
description: .
---

When developing an iPhone app I was recently presented with the challenge of displaying an image and allow for it to be zoomed in and out on while also allowing for a scroll function, up, down and side to side. To accomplish this I used xcode and created my xib with an UIImageView which I placed inside of a UIScrollView like so. (image removed) I wish that was it, but unfortunately there is a bit more to do. We need to add some code to handle the zoom and scroll functions for us. I have compiled and modified this code from various sources, mostly from stackoverflow and I want to give partial credit to[this guide](http://stackoverflow.com/questions/8275234/how-do-i-add-a-scrollable-zoomable-image-into-the-mainview-xib-of-a-utility-base "How do I add a scrollable/zoomable image into the MainView.xib of a Utility Based iPhone Application") which may also help you if my guide is not sufficent. We will be using the image view and scroll view delegates in our .m file so we declare them in the header file like so.

```
<pre lang="php">//
//  ImageViewController.h
//  sfsfum
//
//  Created by MarkusTenghamn on 3/10/13.
//  Copyright (c) 2013 MarkusTenghamn. All rights reserved.
//

#import <UIKit/UIKit.h>

@interface ImageViewController : UIViewController

@property (strong, nonatomic) IBOutlet UIImageView *imgView;
@property (strong, nonatomic) NSString *imgPath;
@property (strong, nonatomic) IBOutlet UIScrollView *scrollView;

- (IBAction)BackBtnPress:(id)sender;

- (void)centerScrollViewContents;
- (void)scrollViewDoubleTapped:(UITapGestureRecognizer*)recognizer;
- (void)scrollViewTwoFingerTapped:(UITapGestureRecognizer*)recognizer;

@end
``` Our .m file will handle all of the touch gestures and the manipulation of the image. I will display my full code here as I believe it is more helpful. Please note that I declare the image path when creating the view outside of this class, see an example of this at the end of the post. ```
<pre lang="php">//
//  ImageViewController.m
//  sfsfum
//
//  Created by MarkusTenghamn on 3/10/13.
//  Copyright (c) 2013 MarkusTenghamn. All rights reserved.
//

#import "ImageViewController.h"

@interface ImageViewController ()

@end

@implementation ImageViewController

@synthesize imgPath = _imgPath;
@synthesize imgView = _imgView;
@synthesize scrollView = _scrollView;

- (id)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil
{
    self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];
    if (self) {
        // Custom initialization
    }
    return self;
}

-(UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView
{
    return self.imgView;
}

- (void)viewWillAppear:(BOOL)animated {
    [super viewWillAppear:animated];
    NSString *path = [[NSBundle mainBundle] pathForResource:self.imgPath ofType:@"png"];
    UIImage *image = [[UIImage alloc] initWithContentsOfFile:path];
    self.imgView.image = image;
    self.imgView.frame = (CGRect){.origin=CGPointMake(0.0f, 0.0f), .size=image.size};
    [self.scrollView addSubview:self.imgView];
    self.scrollView.contentSize = image.size;

    CGRect scrollViewFrame = self.scrollView.frame;
    CGFloat scaleWidth = scrollViewFrame.size.width / self.scrollView.contentSize.width;
    CGFloat scaleHeight = scrollViewFrame.size.height / self.scrollView.contentSize.height;
    CGFloat minScale = MIN(scaleWidth, scaleHeight);
    self.scrollView.minimumZoomScale = minScale;
    self.scrollView.maximumZoomScale = 1.5f;
    self.scrollView.zoomScale = minScale;

    [self centerScrollViewContents];
}

- (void)viewDidLoad
{
    [super viewDidLoad];
    NSString *path = [[NSBundle mainBundle] pathForResource:self.imgPath ofType:@"png"];
    UIImage *image = [[UIImage alloc] initWithContentsOfFile:path];
    self.imgView = [[UIImageView alloc] initWithImage:image];

    UITapGestureRecognizer *doubleTapRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(scrollViewDoubleTapped:)];
    doubleTapRecognizer.numberOfTapsRequired = 2;
    doubleTapRecognizer.numberOfTouchesRequired = 1;
    [self.scrollView addGestureRecognizer:doubleTapRecognizer];

    UITapGestureRecognizer *twoFingerTapRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(scrollViewTwoFingerTapped:)];
    twoFingerTapRecognizer.numberOfTapsRequired = 1;
    twoFingerTapRecognizer.numberOfTouchesRequired = 2;
    [self.scrollView addGestureRecognizer:twoFingerTapRecognizer];
}

- (void)centerScrollViewContents {
    CGSize boundsSize = self.scrollView.bounds.size;
    CGRect contentsFrame = self.imgView.frame;

    if (contentsFrame.size.width < boundsSize.width) {
        contentsFrame.origin.x = (boundsSize.width - contentsFrame.size.width) / 2.0f;
    } else {
        contentsFrame.origin.x = 0.0f;
    }

    if (contentsFrame.size.height < boundsSize.height) {
        contentsFrame.origin.y = (boundsSize.height - contentsFrame.size.height) / 2.0f;
    } else {
        contentsFrame.origin.y = 0.0f;
    }

    self.imgView.frame = contentsFrame;
}

- (void)scrollViewDoubleTapped:(UITapGestureRecognizer*)recognizer {
    // 1
    CGPoint pointInView = [recognizer locationInView:self.imgView];

    // 2
    CGFloat newZoomScale = self.scrollView.zoomScale * 1.5f;
    newZoomScale = MIN(newZoomScale, self.scrollView.maximumZoomScale);

    // 3
    CGSize scrollViewSize = self.scrollView.bounds.size;

    CGFloat w = scrollViewSize.width / newZoomScale;
    CGFloat h = scrollViewSize.height / newZoomScale;
    CGFloat x = pointInView.x - (w / 2.0f);
    CGFloat y = pointInView.y - (h / 2.0f);

    CGRect rectToZoomTo = CGRectMake(x, y, w, h);

    // 4
    [self.scrollView zoomToRect:rectToZoomTo animated:YES];
}

- (void)scrollViewTwoFingerTapped:(UITapGestureRecognizer*)recognizer {
    // Zoom out slightly, capping at the minimum zoom scale specified by the scroll view
    CGFloat newZoomScale = self.scrollView.zoomScale / 1.5f;
    newZoomScale = MAX(newZoomScale, self.scrollView.minimumZoomScale);
    [self.scrollView setZoomScale:newZoomScale animated:YES];
}

- (void)scrollViewDidZoom:(UIScrollView *)scrollView {
    // The scroll view has zoomed, so you need to re-center the contents
    [self centerScrollViewContents];
}

- (void)didReceiveMemoryWarning
{
    [super didReceiveMemoryWarning];
    // Dispose of any resources that can be recreated.
}

- (IBAction)BackBtnPress:(id)sender {
    [self dismissViewControllerAnimated:YES completion:nil];
}
@end
``` Also note that I use png files for my app but you can simply change the extension to any other extension which is supported. To call the image view I use the following code. ```
<pre lang="php">    ImageViewController *view = [ImageViewController alloc];
    view.imgPath = [[self.menus objectAtIndex:[indexPath row]] objectForKey:@"PATH"];
    [self presentViewController:view animated:YES completion:nil];
``` I hope that helps anyone else struggling with this, if you have any questions please do let me know!