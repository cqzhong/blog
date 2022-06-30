---
title: pop动画
urlname: xm0iuq
date: 2018-06-30 14:04:11 +0800
tags: [iOS]
categories: [移动端]
---

`pod 'pop', '~> 1.0.12'`

<!-- more -->

#### kPOPViewScaleXY

```objc
POPBasicAnimation *basicAnimation=[POPBasicAnimation animationWithPropertyNamed:kPOPViewScaleXY];
        basicAnimation.fromValue = [NSValue valueWithCGSize:CGSizeMake(0.8, 0.8)];
        basicAnimation.toValue = [NSValue valueWithCGSize:CGSizeMake(1.3, 1.3)];//1.3倍;
        basicAnimation.duration = 1; //设置动画的间隔时间 默认是0.4秒
        basicAnimation.repeatCount=9999;//HUGE_VALF; //重复次数 HUGE_VALF设置为无限次重复
        [_handImgView pop_addAnimation:basicAnimation forKey:@"basicAnimationKey"];//执行动画
```

#### kPOPViewFrame]

```objc
POPBasicAnimation *pathAnim = [POPBasicAnimation animationWithPropertyNamed:kPOPViewFrame];
        pathAnim.toValue = [NSValue valueWithCGRect:CGRectMake((SCREEN_WIDTH - 57)/2.f, (SCREEN_HEIGHT - 216)/2.f, 57.f, 216.f)];
        [pathAnim setCompletionBlock:^(POPAnimation *anim, BOOL finished) {

            //        [weakSelf.clickScrollImgView setImage:UIImageMake(@"guid_bgScroll")];
            //        weakSelf.clickScrollImgView.transform = CGAffineTransformMakeRotation(AngleWithDegrees(78));

            [_clickScrollImgView removeFromSuperview];
            _clickScrollImgView = nil;
            [weakSelf addSubview:self.bgScrollView];
            [weakSelf.bgScrollView showAScrollAction];
        }];
        pathAnim.duration = 0.35f;
        [weakSelf.clickScrollImgView pop_addAnimation:pathAnim forKey:@"pathAnim"];
```

#### kPOPViewCenter

```objc

POPBasicAnimation *pathAnim = [POPBasicAnimation animationWithPropertyNamed:kPOPViewCenter];
    pathAnim.toValue = [NSValue valueWithCGPoint:self.center];
    [pathAnim setCompletionBlock:^(POPAnimation *anim, BOOL finished) {

    }];
    pathAnim.duration = 1.f;
    [_clickScrollImgView pop_addAnimation:pathAnim forKey:@"pathAnim"];
```

#### 数字动画

```objc
#pragma mark - Private Method
- (void)numeralBeatingAnim:(NSString *)balance {

    POPAnimatableProperty *prop = [POPAnimatableProperty propertyWithName:@"money" initializer:^(POPMutableAnimatableProperty *prop) {

        prop.writeBlock = ^(id obj, const CGFloat values[]) {
            UILabel *label = (UILabel *)obj;

            @autoreleasepool {
                NSNumberFormatter *formatter = [[NSNumberFormatter alloc] init];
                [formatter setNumberStyle:NSNumberFormatterNoStyle];
                [formatter setFormatWidth:9];
                [formatter setPositiveFormat:@",##0"];
                NSString *titleStr =  [NSString stringWithFormat:@"%.@",[formatter stringFromNumber:@(values[0])]];
                [label setText:titleStr];
            }
        };
        //力学阀值,值越大writeBlock的调用次数越少
        prop.threshold = 1;
    }];

    POPBasicAnimation *anBasic = [POPBasicAnimation easeInEaseOutAnimation];
    anBasic.property = prop;
    anBasic.fromValue = @(0.00);
    anBasic.toValue = @([balance integerValue]);
    anBasic.duration = 1.4f;
    [_balanceLabel pop_addAnimation:anBasic forKey:@"money"];
}
```

#### 属性

```objc
NSString * const kPOPLayerBackgroundColor = @"backgroundColor";  //背景色
NSString * const kPOPLayerBounds = @"bounds";  //坐标
NSString * const kPOPLayerCornerRadius = @"cornerRadius";  //圆形 值越大,角就越圆
NSString * const kPOPLayerBorderWidth = @"borderWidth";  //边框宽度
NSString * const kPOPLayerBorderColor = @"borderColor";  //边框色
NSString * const kPOPLayerOpacity = @"opacity"; //透明度
NSString * const kPOPLayerPosition = @"position"; //位置 position是相对于屏幕的
NSString * const kPOPLayerPositionX = @"positionX";
NSString * const kPOPLayerPositionY = @"positionY";
NSString * const kPOPLayerRotation = @"rotation"; //旋转
NSString * const kPOPLayerRotationX = @"rotationX";
NSString * const kPOPLayerRotationY = @"rotationY";
NSString * const kPOPLayerScaleX = @"scaleX"; //缩放系数
NSString * const kPOPLayerScaleXY = @"scaleXY"; //XY缩放系数
NSString * const kPOPLayerScaleY = @"scaleY"; //Y缩放系数
NSString * const kPOPLayerSize = @"size";  //大小
NSString * const kPOPLayerSubscaleXY = @"subscaleXY";
NSString * const kPOPLayerSubtranslationX = @"subtranslationX";
NSString * const kPOPLayerSubtranslationXY = @"subtranslationXY";
NSString * const kPOPLayerSubtranslationY = @"subtranslationY";
NSString * const kPOPLayerSubtranslationZ = @"subtranslationZ";
NSString * const kPOPLayerTranslationX = @"translationX"; //X轴平移量
NSString * const kPOPLayerTranslationXY = @"translationXY"; //XY轴平移量
NSString * const kPOPLayerTranslationY = @"translationY"; //Y轴平移量
NSString * const kPOPLayerTranslationZ = @"translationZ"; //Z轴平移量
NSString * const kPOPLayerZPosition = @"zPosition";  //遮挡属性
NSString * const kPOPLayerShadowColor = @"shadowColor"; //设置阴影
NSString * const kPOPLayerShadowOffset = @"shadowOffset"; //阴影偏移
NSString * const kPOPLayerShadowOpacity = @"shadowOpacity"; //阴影透明度
NSString * const kPOPLayerShadowRadius = @"shadowRadius"; //阴影半径

// CAShapeLayer
NSString * const kPOPShapeLayerStrokeStart = @"shapeLayer.strokeStart";//strokeStart  动画的fromValue = 0，toValue = 1 表示从路径的0位置画到1 怎么画是按照清除开始的位置也就是清除0 一直清除到1 效果就是一条路径慢慢的消失  strokeStart  动画的fromValue = 1，toValue = 0 表示从路径的1位置画到0 怎么画是按照清除开始的位置也就是1 这样开始的路径是空的（即都被清除掉了）一直清除到0 效果就是一条路径被反方向画出来
NSString * const kPOPShapeLayerStrokeEnd = @"shapeLayer.strokeEnd";// strokeEnd  动画的fromValue = 0，toValue = 1  表示 这里我们分3个点说明动画的顺序  strokeEnd从结尾开始清除 首先整条路径先清除后2/3，接着清除1/3 效果就是正方向画出路径     strokeEnd  动画的fromValue = 1，toValue = 0 效果就是反方向路径慢慢消失
NSString * const kPOPShapeLayerStrokeColor = @"shapeLayer.strokeColor";  //画笔的色
NSString * const kPOPShapeLayerFillColor = @"shapeLayer.fillColor";
NSString * const kPOPShapeLayerLineWidth = @"shapeLayer.lineWidth"; //线的宽度
NSString * const kPOPShapeLayerLineDashPhase = @"shapeLayer.lineDashPhase";

复制代码
```
