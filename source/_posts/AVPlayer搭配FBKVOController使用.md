---
title: AVPlayer搭配FBKVOController使用
urlname: oxhb6m
date: 2018-03-29 19:26:04 +0800
tags: [AVPlayer,iOS]
categories: [移动端]
---

1、导入

```objectivec
#import <AVKit/AVKit.h>
#import <AVFoundation/AVFoundation.h>
#import <KVOController/KVOController.h>
```

<!-- more -->

```objc

@property (nonatomic, strong) AVPlayer *player;
@property (nonatomic, strong) AVPlayerLayer *playerLayer;
@property (nonatomic, strong) NSURL *playUrl;
@property (nonatomic, strong) UIActivityIndicatorView *loadingView;

    [self addSubview:self.loadingView];
    [self.loadingView mas_makeConstraints:^(MASConstraintMaker *make) {

        make.center.equalTo(self);
        make.height.width.mas_offset(CDREALVALUE_HEIGHT(30.0));
    }];

    // 播放器
    self.playUrl = [NSURL URLWithString:model.pinyin_audio];
    [self.layer addSublayer:self.playerLayer];
    [self.player play];
    self.playerLayer.frame = CGRectMake(0, 0, CDREALVALUE_HEIGHT(320), CDREALVALUE_WIDTH(180));

    [self.loadingView startAnimating];




// 获取播放器状态
- (void)avPlayerItemStatus:(AVPlayerItemStatus)status {

    switch (status) {
        case AVPlayerItemStatusReadyToPlay: {


            break;
        }
        case AVPlayerItemStatusUnknown: {

            break;
        }
        case AVPlayerItemStatusFailed: {


            break;
        }
        default:
            break;
    }

}
// 播放完成
- (void)didPlayToEnd {

    // 播放完成并回到首帧
    [self.player seekToTime:kCMTimeZero];

}
// 释放播放器
- (void)releasePlayer {
    if (_player) {
        [_player pause];
        [_playerLayer removeFromSuperlayer];

        [self.KVOController unobserveAll];
        [[NSNotificationCenter defaultCenter] removeObserver:AVPlayerItemDidPlayToEndTimeNotification];
        _player = nil;
        _playerLayer = nil;
    }
}



- (AVPlayer *)player {
    if (!_player) {
        _player = [[AVPlayer alloc] initWithURL:self.playUrl];
        [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(didPlayToEnd) name:AVPlayerItemDidPlayToEndTimeNotification object:_player.currentItem];
//        _player.volume = 1;
    }
    return _player;
}
- (AVPlayerLayer *)playerLayer {
    if (!_playerLayer) {
        _playerLayer = [AVPlayerLayer playerLayerWithPlayer:self.player];
        //AVLayerVideoGravityResizeAspect
        _playerLayer.videoGravity = AVLayerVideoGravityResizeAspectFill;

        @weakify(self);
        self.KVOController = [FBKVOController controllerWithObserver:self];
        [self.KVOController observe:self.player.currentItem keyPath:@keypath(self.player.currentItem, status) options:NSKeyValueObservingOptionNew block:^(id  _Nullable observer, id  _Nonnull object, NSDictionary<NSString *,id> * _Nonnull change) {

            @strongify(self);
            AVPlayerItemStatus state = [change[NSKeyValueChangeNewKey] integerValue];
            [self avPlayerItemStatus:state];
        }];

        [self.KVOController observe:self.player keyPath:@keypath(self.player, rate) options:NSKeyValueObservingOptionNew block:^(id  _Nullable observer, id  _Nonnull object, NSDictionary<NSString *,id> * _Nonnull change) {

            @strongify(self);
            CGFloat rate = [change[NSKeyValueChangeNewKey] floatValue];

            CDDLog(@"rate: %.2f", rate);
            if (rate > 0.0) { // 正在播放

                [self.loadingView stopAnimating];
            } else { // 暂停
                [self.loadingView startAnimating];
            }
        }];

    }
    return _playerLayer;
}
- (UIActivityIndicatorView *)loadingView {
    if (!_loadingView) {
        _loadingView = [[UIActivityIndicatorView alloc] initWithActivityIndicatorStyle:UIActivityIndicatorViewStyleWhite];
        _loadingView.hidesWhenStopped = true;
    }
    return _loadingView;
}
```
