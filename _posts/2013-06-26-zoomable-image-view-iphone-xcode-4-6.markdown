---
layout: post
title:  "Zoomable image view for iPhone in Xcode 4.6"
redirect_from:
   - /zoomable-image-view-iphone-xcode-4-6
date:   2013-06-26 23:24:49 +0100
categories: best domain registrar
description: When developing an iPhone app I was recently presented with the challenge of displaying an image and allow for it to be zoomed in and out on while also allowing for a scroll function, up, down and sid...
---

**This post has been migrated and imported into different systems over the years, I have not had a chance to format this post so it may be hard to read but I have left it here as a reference.**

When developing an iPhone app I was recently presented with the challenge of displaying an image and allow for it to be zoomed in and out on while also allowing for a scroll function, up, down and side to side. To accomplish this I used xcode and created my xib with an UIImageView which I placed inside of a UIScrollView like so.  
  
 (image removed)   
  
   
  
 I wish that was it, but unfortunately there is a bit more to do. We need to add some code to handle the zoom and scroll functions for us. I have compiled and modified this code from various sources, mostly from stackoverflow and I want to give partial credit to[this guide](http://stackoverflow.com/questions/8275234/how-do-i-add-a-scrollable-zoomable-image-into-the-mainview-xib-of-a-utility-base "How do I add a scrollable/zoomable image into the MainView.xib of a Utility Based iPhone Application") which may also help you if my guide is not sufficent.  
  
 We will be using the image view and scroll view delegates in our .m file so we declare them in the header file like so.

```
<pre lang="php">//<br></br>
//  ImageViewController.h<br></br>
//  sfsfum<br></br>
//<br></br>
//  Created by MarkusTenghamn on 3/10/13.<br></br>
//  Copyright (c) 2013 MarkusTenghamn. All rights reserved.<br></br>
//<br></br><br></br>
#import <UIKit/UIKit.h><br></br><br></br>
@interface ImageViewController : UIViewController<br></br><br></br>
@property (strong, nonatomic) IBOutlet UIImageView *imgView;<br></br>
@property (strong, nonatomic) NSString *imgPath;<br></br>
@property (strong, nonatomic) IBOutlet UIScrollView *scrollView;<br></br><br></br>
- (IBAction)BackBtnPress:(id)sender;<br></br><br></br>
- (void)centerScrollViewContents;<br></br>
- (void)scrollViewDoubleTapped:(UITapGestureRecognizer*)recognizer;<br></br>
- (void)scrollViewTwoFingerTapped:(UITapGestureRecognizer*)recognizer;<br></br><br></br>
@end
```  
   
  
 Our .m file will handle all of the touch gestures and the manipulation of the image. I will display my full code here as I believe it is more helpful. Please note that I declare the image path when creating the view outside of this class, see an example of this at the end of the post.  
```
<pre lang="php">//<br></br>
//  ImageViewController.m<br></br>
//  sfsfum<br></br>
//<br></br>
//  Created by MarkusTenghamn on 3/10/13.<br></br>
//  Copyright (c) 2013 MarkusTenghamn. All rights reserved.<br></br>
//<br></br><br></br>
#import "ImageViewController.h"<br></br><br></br>
@interface ImageViewController ()<br></br><br></br>
@end<br></br><br></br>
@implementation ImageViewController<br></br><br></br>
@synthesize imgPath = _imgPath;<br></br>
@synthesize imgView = _imgView;<br></br>
@synthesize scrollView = _scrollView;<br></br><br></br>
- (id)initWithNibName:(NSString *)nibNameOrNil bundle:(NSBundle *)nibBundleOrNil<br></br>
{<br></br>
    self = [super initWithNibName:nibNameOrNil bundle:nibBundleOrNil];<br></br>
    if (self) {<br></br>
        // Custom initialization<br></br>
    }<br></br>
    return self;<br></br>
}<br></br><br></br>
-(UIView *)viewForZoomingInScrollView:(UIScrollView *)scrollView<br></br>
{<br></br>
    return self.imgView;<br></br>
}<br></br><br></br>
- (void)viewWillAppear:(BOOL)animated {<br></br>
    [super viewWillAppear:animated];<br></br>
    NSString *path = [[NSBundle mainBundle] pathForResource:self.imgPath ofType:@"png"];<br></br>
    UIImage *image = [[UIImage alloc] initWithContentsOfFile:path];<br></br>
    self.imgView.image = image;<br></br>
    self.imgView.frame = (CGRect){.origin=CGPointMake(0.0f, 0.0f), .size=image.size};<br></br>
    [self.scrollView addSubview:self.imgView];<br></br>
    self.scrollView.contentSize = image.size;<br></br><br></br>
    CGRect scrollViewFrame = self.scrollView.frame;<br></br>
    CGFloat scaleWidth = scrollViewFrame.size.width / self.scrollView.contentSize.width;<br></br>
    CGFloat scaleHeight = scrollViewFrame.size.height / self.scrollView.contentSize.height;<br></br>
    CGFloat minScale = MIN(scaleWidth, scaleHeight);<br></br>
    self.scrollView.minimumZoomScale = minScale;<br></br>
    self.scrollView.maximumZoomScale = 1.5f;<br></br>
    self.scrollView.zoomScale = minScale;<br></br><br></br>
    [self centerScrollViewContents];<br></br>
}<br></br><br></br>
- (void)viewDidLoad<br></br>
{<br></br>
    [super viewDidLoad];<br></br>
    NSString *path = [[NSBundle mainBundle] pathForResource:self.imgPath ofType:@"png"];<br></br>
    UIImage *image = [[UIImage alloc] initWithContentsOfFile:path];<br></br>
    self.imgView = [[UIImageView alloc] initWithImage:image];<br></br><br></br>
    UITapGestureRecognizer *doubleTapRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(scrollViewDoubleTapped:)];<br></br>
    doubleTapRecognizer.numberOfTapsRequired = 2;<br></br>
    doubleTapRecognizer.numberOfTouchesRequired = 1;<br></br>
    [self.scrollView addGestureRecognizer:doubleTapRecognizer];<br></br><br></br>
    UITapGestureRecognizer *twoFingerTapRecognizer = [[UITapGestureRecognizer alloc] initWithTarget:self action:@selector(scrollViewTwoFingerTapped:)];<br></br>
    twoFingerTapRecognizer.numberOfTapsRequired = 1;<br></br>
    twoFingerTapRecognizer.numberOfTouchesRequired = 2;<br></br>
    [self.scrollView addGestureRecognizer:twoFingerTapRecognizer];<br></br>
}<br></br><br></br>
- (void)centerScrollViewContents {<br></br>
    CGSize boundsSize = self.scrollView.bounds.size;<br></br>
    CGRect contentsFrame = self.imgView.frame;<br></br><br></br>
    if (contentsFrame.size.width < boundsSize.width) {<br></br>
        contentsFrame.origin.x = (boundsSize.width - contentsFrame.size.width) / 2.0f;<br></br>
    } else {<br></br>
        contentsFrame.origin.x = 0.0f;<br></br>
    }<br></br><br></br>
    if (contentsFrame.size.height < boundsSize.height) {<br></br>
        contentsFrame.origin.y = (boundsSize.height - contentsFrame.size.height) / 2.0f;<br></br>
    } else {<br></br>
        contentsFrame.origin.y = 0.0f;<br></br>
    }<br></br><br></br>
    self.imgView.frame = contentsFrame;<br></br>
}<br></br><br></br>
- (void)scrollViewDoubleTapped:(UITapGestureRecognizer*)recognizer {<br></br>
    // 1<br></br>
    CGPoint pointInView = [recognizer locationInView:self.imgView];<br></br><br></br>
    // 2<br></br>
    CGFloat newZoomScale = self.scrollView.zoomScale * 1.5f;<br></br>
    newZoomScale = MIN(newZoomScale, self.scrollView.maximumZoomScale);<br></br><br></br>
    // 3<br></br>
    CGSize scrollViewSize = self.scrollView.bounds.size;<br></br><br></br>
    CGFloat w = scrollViewSize.width / newZoomScale;<br></br>
    CGFloat h = scrollViewSize.height / newZoomScale;<br></br>
    CGFloat x = pointInView.x - (w / 2.0f);<br></br>
    CGFloat y = pointInView.y - (h / 2.0f);<br></br><br></br>
    CGRect rectToZoomTo = CGRectMake(x, y, w, h);<br></br><br></br>
    // 4<br></br>
    [self.scrollView zoomToRect:rectToZoomTo animated:YES];<br></br>
}<br></br><br></br>
- (void)scrollViewTwoFingerTapped:(UITapGestureRecognizer*)recognizer {<br></br>
    // Zoom out slightly, capping at the minimum zoom scale specified by the scroll view<br></br>
    CGFloat newZoomScale = self.scrollView.zoomScale / 1.5f;<br></br>
    newZoomScale = MAX(newZoomScale, self.scrollView.minimumZoomScale);<br></br>
    [self.scrollView setZoomScale:newZoomScale animated:YES];<br></br>
}<br></br><br></br>
- (void)scrollViewDidZoom:(UIScrollView *)scrollView {<br></br>
    // The scroll view has zoomed, so you need to re-center the contents<br></br>
    [self centerScrollViewContents];<br></br>
}<br></br><br></br>
- (void)didReceiveMemoryWarning<br></br>
{<br></br>
    [super didReceiveMemoryWarning];<br></br>
    // Dispose of any resources that can be recreated.<br></br>
}<br></br><br></br>
- (IBAction)BackBtnPress:(id)sender {<br></br>
    [self dismissViewControllerAnimated:YES completion:nil];<br></br>
}<br></br>
@end
```  
   
  
 Also note that I use png files for my app but you can simply change the extension to any other extension which is supported. To call the image view I use the following code.  
```
<pre lang="php">    ImageViewController *view = [ImageViewController alloc];<br></br>
    view.imgPath = [[self.menus objectAtIndex:[indexPath row]] objectForKey:@"PATH"];<br></br>
    [self presentViewController:view animated:YES completion:nil];
```  
   
  
 I hope that helps anyone else struggling with this, if you have any questions please do let me know!