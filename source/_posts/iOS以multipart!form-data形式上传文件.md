---
title: iOS以multipart/form-data形式上传文件
urlname: cdcafa
date: 2018-04-28 21:42:05 +0800
tags: [iOS]
categories: [移动端]
---

这里以 YTKRequest 为例，
boundary 仅作分割参数作用，越复杂越好，其余无要求

<!-- more -->

```objectivec
- (NSURLRequest *)buildCustomUrlRequest {

    NSMutableData *body = [NSMutableData data];

    NSString *BOUNDARY = @"0xKhTmLbOuNdArY";
    /** 遍历字典将字典中的键值对转换成请求格式:

     --Boundary+72D4CD655314C423

     Content-Disposition: form-data; name="empId"
     254

     --Boundary+72D4CD655314C423

     Content-Disposition: form-data; name="shopId"

     18718

     */

    //表单数据 param

    [self.params enumerateKeysAndObjectsUsingBlock:^(id key, id obj, BOOL *stop) {

        NSMutableString *fieldStr = [NSMutableString string];

        [fieldStr appendString:[NSString stringWithFormat:@"--%@\r\n", BOUNDARY]];

        [fieldStr appendString:[NSString stringWithFormat:@"Content-Disposition: form-data; name=\"%@\"\r\n\r\n", key]];

        [fieldStr appendString:[NSString stringWithFormat:@"%@", obj]];

        NSLog(@"file=====%@",fieldStr);

        [body appendData:[fieldStr dataUsingEncoding:NSUTF8StringEncoding]];

//        [body appendData:[obj dataUsingEncoding:NSUTF8StringEncoding]];

//        NSData *data = [NSKeyedArchiver archivedDataWithRootObject:obj];

//        [body appendData:data];

        [body appendData:[@"\r\n" dataUsingEncoding:NSUTF8StringEncoding]];

    }];




    //文件逻辑

    for (NSDictionary *fileDictionary in self.imageDictionaryArray) {

        NSString *fileName = fileDictionary[photoImageNameTag];


        UIImage *image  = fileDictionary[photoImageTag];

        NSData *data    = UIImageJPEGRepresentation(image, 0.5);

        NSString *name = BimImageGuidName;

        NSString *type = @"image";




        NSString *param = [NSString stringWithFormat:@"--%@\r\nContent-Disposition: form-data; name=\"%@\";filename=\"%@\"\r\nContent-Type: application/octet-stream\r\n\r\n",BOUNDARY,[NSString stringWithFormat:@"%@%@",name,type],fileName,nil];

        [body appendData:[param dataUsingEncoding:NSUTF8StringEncoding]];

        [body appendData:data];

        [body appendData:[@"\r\n" dataUsingEncoding:NSUTF8StringEncoding]];

    }



    for (NSDictionary *fileDictionary in self.amrDictionaryArray) {


        NSString *filePath = fileDictionary[recordPathTag];

        NSString *fileName = [NSString stringWithFormat:@"%@.amr",fileDictionary[recordNameTag]];

        NSData *data    = [[NSData alloc]initWithContentsOfURL:[NSURL fileURLWithPath:filePath]];

        NSString *name = BIMAmrGuidName;

        NSString *type = @"amr";

        NSString *param = [NSString stringWithFormat:@"--%@\r\nContent-Disposition: form-data; name=\"%@\";filename=\"%@\"\r\nContent-Type: application/octet-stream\r\n\r\n",BOUNDARY,[NSString stringWithFormat:@"%@%@",name,type],fileName,nil];

        [body appendData:[param dataUsingEncoding:NSUTF8StringEncoding]];

        [body appendData:data];

        [body appendData:[@"\r\n" dataUsingEncoding:NSUTF8StringEncoding]];

    }

    NSMutableURLRequest *request = [NSMutableURLRequest requestWithURL:[NSURL URLWithString:[NSString stringWithFormat:@"%@%@",[YTKNetworkConfig sharedConfig].baseUrl,self.requestUrl]]];

    NSString *endString = [NSString stringWithFormat:@"--%@--",BOUNDARY];

    [body appendData:[endString dataUsingEncoding:NSUTF8StringEncoding]];

    [request setHTTPBody:body];

    // 设置请求类型为post请求

    request.HTTPMethod = @"post";

    // 设置request的请求体

    request.HTTPBody = body;

    // 设置头部数据，标明上传数据总大小，用于服务器接收校验

    [request setValue:@(body.length).stringValue forHTTPHeaderField:@"Content-Length"];

    [request setValue:[BIMSQLiteManager manager].cookieToken forHTTPHeaderField:BIMRequestKeyCookie];

    // 设置头部数据，指定了http post请求的编码方式为multipart/form-data（上传文件必须用这个）。

    [request setValue:[NSString stringWithFormat:@"multipart/form-data; boundary=%@",BOUNDARY] forHTTPHeaderField:@"Content-Type"];

    //token 放请求头
//    [request setValue:@"" forHTTPHeaderField:@"Authorization"];

    return request;
}
```
