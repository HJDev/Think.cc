---
title: "iOS 获取图片的原始格式"
date: 2017-09-28T17:16:29+08:00
draft: false
slug: "ios-huo-qu-tu-pian-de-yuan-shi-ge-shi"
tags: ["iOS"]
---

- [ ] show list
- [x] next list

今天测试给过来一张图片(后缀是.png)说无法在 APP 的 WebView 里面无法显示，而且在 Safari 里也是无法打开的，但在谷歌浏览器上是可以正常显示。起初是知道 [WebP](https://zh.wikipedia.org/wiki/WebP) 格式的图片苹果是不支持显示的，但这个图片的后缀是.png 的，难道还有 png 的图片是苹果不支持的么？

根据个人经验，是没有听说苹果不支持 png 格式的图片的，这时想到以前自己更改 JPG 图片后缀的事情，是不是这张图片也是经过别人手动改后缀的呢，带着这个疑问，我决定手动判断这张图片的原始格式。

<!--more-->

记得以前在看 [SDWebImage](https://github.com/rs/SDWebImage) 源码时，源码中是有关于判断根据 image 来判断图片的实际格式的，于是从 SDWebImage 中的源码中抠出来判断图片实际格式的代码。

> 图片的前8位是存储图片格式的，可以通过先读取图片的数据，拿到图片的前8位来判断图片的类型

```objective-c
/**
 *
 * 根据图片数据获取图片的原始类型
 *
 * @param data 图片的二进制数据
 * @return 图片的实际格式
 */
- (NSString *)typeForImageData:(NSData *)data {
    uint8_t c;
    [data getBytes:&c length:1];

    switch (c) {
        case 0xFF:
            return @"image/jpeg";
        case 0x89:
            return @"image/png";
        case 0x47:
            return @"image/gif";
        case 0x49:
        case 0x4D:
            return @"image/tiff";
        case 0x52: {
            //R as RIFF for WEBP
            if (data.length < 12) {
                return nil;
            }
            NSString *identifierTypeStr = [[NSString alloc] initWithData:[data subdataWithRange:NSMakeRange(0, 12)] encoding:NSASCIIStringEncoding];
            if ([identifierTypeStr hasPrefix:@"RIFF"] && [identifierTypeStr hasSuffix:@"WEBP"]) {
                return @"image/webp";
            }
            return nil;
        }

        default:
            break;
    }
    return nil;
}
```

That's All.

[Code](https://github.com/HJDev/HJEXIF.git)

原文链接:[iOS 获取图片的原始格式](https://www.teamleader.cn/ios-huo-qu-tu-pian-de-yuan-shi-ge-shi/)
