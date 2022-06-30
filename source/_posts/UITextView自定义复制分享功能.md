---
title: UITextView自定义复制分享功能
urlname: mv7i07
date: 2018-06-13 21:41:04 +0800
tags: [iOS]
categories: [移动端]
---

自定义 UIMenuController，实现下图效果

<!-- more -->

![](https://cdn.nlark.com/yuque/0/2020/jpeg/1028501/1602936495378-42255d22-8baf-4bc0-a374-f89fc45d8da0.jpeg#align=left&display=inline&height=667&margin=%5Bobject%20Object%5D&originHeight=1334&originWidth=750&size=0&status=done&style=none&width=375)

```objc

@interface CDRichTextView : UITextView



-(UIViewController *)cd_viewController;

@end


#import "CDRichTextView.h"
#define kRootViewController [UIApplication sharedApplication].keyWindow.rootViewController

@implementation CDRichTextView

- (instancetype)initWithFrame:(CGRect)frame {

    if (self = [super initWithFrame:frame]) {

        [self setEditable:false];
        self.scrollEnabled = false;
        self.textAlignment = NSTextAlignmentJustified;

        //        self.textContainer.lineFragmentPadding = 0.0;
//                self.textContainer.lineBreakMode = NSLineBreakByTruncatingTail;
        CGFloat padding = self.textContainer.lineFragmentPadding;
        self.textContainerInset = UIEdgeInsetsMake(-padding, 0, 2*padding + 1, 0);
    }
    return self;
}

- (BOOL)canBecomeFirstResponder {

    return true;
}

- (BOOL)canPerformAction:(SEL)action withSender:(id)sender {

    UIMenuController *menuController = [UIMenuController sharedMenuController];
    UIMenuItem *copyItem = [[UIMenuItem alloc] initWithTitle:@"复制" action:@selector(copyEvent:)];
    UIMenuItem *sahreItem = [[UIMenuItem alloc] initWithTitle:@"分享" action:@selector(shareEvent:)];
    menuController.menuItems = @[copyItem, sahreItem];

    return (action == @selector(copyEvent:) ||
            action == @selector(shareEvent:));
}

- (void)copyEvent:(id)sender {
    [self copy:sender];
}
- (void)shareEvent:(id)sender {

    NSString *content = [self.attributedText.string substringWithRange:self.selectedRange];
    if (content.length == 0) return;

    UIActivityViewController *activityVC = [[UIActivityViewController alloc]initWithActivityItems:@[content] applicationActivities:nil];
    activityVC.excludedActivityTypes = @[UIActivityTypeAirDrop];

    activityVC.completionWithItemsHandler = ^(NSString *activityType,BOOL completed,NSArray *returnedItems,NSError *activityError) {

        CDDLog(@"activityType :%@", activityType);

//        activityVC = nil;
        if (completed) {
            CDDLog(@"completed");
        } else {
            CDDLog(@"cancel");
        }
    };

    [self.cd_viewController presentViewController:activityVC animated:true completion:^{
    }];
}

-(UIViewController *)cd_viewController {
    id responder = self;
    while (responder){
        if ([responder isKindOfClass:[UIViewController class]]){
            return responder;
        }
        responder = [responder nextResponder];
    }
    return nil;
}

@end
```
