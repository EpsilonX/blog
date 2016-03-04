---
title: 使用iOS自带CommonCrypto库计算MD5 
date: 2016-03-04 10:36:12
tags: 
- iOS
- CommonCrypto
- MD5
category: iOS开发
---

开发中常常会用到MD5计算，实际上iOS 5.0就已经内置了[加密和哈希计算的库](https://developer.apple.com/library/mac/documentation/Security/Conceptual/cryptoservices/GeneralPurposeCrypto/GeneralPurposeCrypto.html)，这里就简单讲下用其中的[CommonCrypto](https://developer.apple.com/library/mac/documentation/Darwin/Reference/ManPages/man3/Common%20Crypto.3cc.html#//apple_ref/doc/man/3cc/CommonCrypto
)进行MD5计算。

Objective-C代码:
```Objective-C
#import <CommonCrypto/CommonCrypto.h>
+(NSString *)MD5:(NSString *)str
{
    const char* cstr = [str UTF8String];
    unsigned char result[CC_MD5_DIGEST_LENGTH];
    CC_MD5(cstr, (CC_LONG)strlen(cstr), result);
    
    return [NSString stringWithFormat:
            @"%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x%02x",
            result[0], result[1], result[2], result[3], result[4], result[5], result[6], result[7],
            result[8], result[9], result[10], result[11], result[12], result[13], result[14], result[15]];
}

```

此外，CommonCrypto还支持SHA，Hmac等。



