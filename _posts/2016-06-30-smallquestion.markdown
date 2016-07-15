---
layout: post
title: "small-question"
date: "2016-06-30 16:52:49 +0800"
---
===============================================================

今天遇到一个奇怪的问题，用AVAudioPlayer音频播放器播放音频的时候，突然无法播放，一直报如下错误：

Error : >aq> 1605: failed (-66680); will stop (11025/0 frames)

调了半天未果，原来是模拟器抽风了，把程序卸掉，xCode、模拟器关掉重启，然后就正常了

===============================================================

关于BOOL类型的值赋值的问题

        BOOL isNeedLogin = [urlInfos objectAtIndex:2];

编译没有报错，但是在使用的时候获得的值一直是YES，打印urlInfos值出现的是NO

正确赋值方法
        BOOL isNeedLogin = [[urlInfos objectAtIndex:2] boolValue];

===============================================================
