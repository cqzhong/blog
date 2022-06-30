---
title: YTKNetwork 的使用总结(自定义请求、上传图片、取消请求)
urlname: hndor7
date: 2018-09-08 12:58:06 +0800
tags: [YTKNetwork,iOS]
categories: [移动端]
---

### YTKNetwork 的配置

<!-- more -->

```objc

//MARK: - 配置请求
- (void)configurationNetwork:(BHContext *)context {

YTKNetworkConfig *config = [YTKNetworkConfig sharedConfig];

//根据环境配置请求的IP，可以在工程内获取baseURL： [YTKNetworkConfig sharedConfig].baseUrl;
if (context.env == BHEnvironmentDev) {

config.baseUrl = kRequestURLBaseTest;

} else {

config.baseUrl = kRequestURLBaseProduct;

}

//       config.cdnUrl = @"";

//过滤非法请求
[config addUrlFilter:[HPRequestURLFilter filter]];

YTKNetworkAgent *agent = [YTKNetworkAgent sharedAgent];

//请求响应参数的类型
[agent setValue: [NSSet setWithObjects:@"application/json", @"text/html",@"text/json",@"text/javascript", @"text/plain",@"text/xml",@"image/*", nil]

forKeyPath:@"jsonResponseSerializer.acceptableContentTypes"];
}
```

### 取消请求

```objc

[[YTKNetworkAgent sharedAgent] cancelAllRequests];
[[YTKNetworkAgent sharedAgent] cancelRequest:];
```

### 上传图片（单张、多张）

```objc

#import "HPBaseRequest.h"

#import "HPImageInfoModel.h"

// HPBaseRequest继承 YTKRequest 里面添加了一些自定义的方法。
@interface HPHeadUploadRequest : HPBaseRequest <HPImageInfoModel *>

+ (instancetype)requestWithImage:(UIImage *)image delegate:(id<YTKRequestDelegate>)delegate;

@end

#import "HPHeadUploadRequest.h"

#import "AFNetworking.h"

@interface HPHeadUploadRequest ()

@property (nonatomic, strong) UIImage *image;

@end

@implementation HPHeadUploadRequest

+ (instancetype)requestWithImage:(UIImage *)image delegate:(id<YTKRequestDelegate>)delegate {

HPHeadUploadRequest *request = [[HPHeadUploadRequest alloc] initWithParams:@{}];

request.delegate = delegate;

request.image = image;

[request start];

return request;
}

- (NSMutableDictionary *)defaultParams {

return [NSMutableDictionary dictionaryWithObjectsAndKeys: HPRequestAliyunOSSAccount,HPRequestKeyPath ,HPRequestKeyImageType,HPRequestKeyType, nil];

}

- (NSString *)requestUrl {
return @"service-uploads-uploadfile.server";

}

- (AFConstructingBlock)constructingBodyBlock {

return ^(id<AFMultipartFormData> formData) {

NSDateFormatter *formatter = [[NSDateFormatter alloc] init];

formatter.dateFormat = @"yyyyMMddHHmmss";

NSString *str = [formatter stringFromDate:[NSDate date]];

NSString *fileName = [NSString stringWithFormat:@"%@.jpg", str];

NSData *data = UIImageJPEGRepresentation(self.image, 0.5);

NSString *name = HPRequestKeyFileName;

NSString *type = @"image/jpeg/png/jpg";

[formData appendPartWithFileData:data name:name fileName:fileName mimeType:type];

};
}


- (YTKRequestMethod)requestMethod {

return YTKRequestMethodPOST;
}

- (BOOL)contentIsArray {

return false;
}

- (Class)contentType {

return [HPImageInfoModel class];
}

@end
```

多张图片上传

```objc

#import "HPBaseRequest.h"

#import "HPImageInfoModel.h"

@interface HPBatchUploadRequest : HPBaseRequest

+ (instancetype)requestWithImage:(NSArray<UIImage *> *)imageM delegate:(id<YTKRequestDelegate>)delegate;

@end


#import "HPBatchUploadRequest.h"

#import "AFNetworking.h"

@interface HPBatchUploadRequest ()

@property (nonatomic, strong) NSArray <UIImage *> *imageM;

@end


@implementation HPBatchUploadRequest


+ (instancetype)requestWithImage:(NSArray<UIImage *> *)imageM delegate:(id<YTKRequestDelegate>)delegate {

HPBatchUploadRequest *request = [[HPBatchUploadRequest alloc] initWithParams:@{}];

request.delegate = delegate;

request.imageM = imageM;

[request start];

return request;

}

- (NSMutableDictionary *)defaultParams {

return [NSMutableDictionary dictionaryWithObjectsAndKeys: HPRequestAliyunOSSAccount,HPRequestKeyPath ,HPRequestKeyImageType,HPRequestKeyType, nil];
}

- (NSString *)requestUrl {

return @"service-uploads-batchuploadfile.server";
}

- (AFConstructingBlock)constructingBodyBlock {

return ^(id<AFMultipartFormData> formData) {
for (int i = 0;i <[self.imageM count] ;i ++) {


NSDateFormatter *formatter = [[NSDateFormatter alloc] init];

formatter.dateFormat = @"yyyyMMddHHmmss";

NSString *str = [formatter stringFromDate:[NSDate date]];

NSString *fileName = [NSString stringWithFormat:@"%@.jpg", str];



UIImage *image  = self.imageM[i];

NSData *data    = UIImageJPEGRepresentation(image, 0.5);

NSString *name = HPRequestKeyMoreFileName;

NSString *type = @"image/jpeg/png/jpg";

[formData appendPartWithFileData:data name:name fileName:fileName mimeType:type];

}

};

}


- (YTKRequestMethod)requestMethod {

return YTKRequestMethodPOST;
}

- (BOOL)contentIsArray {

return false;

}

@end
```

### YTKNetwork 自定义 request

```objc
#import "BIMBaseRequest.h"

typedef NS_ENUM(NSInteger, BIMUploadRequestStatus) {

BIMUploadRequestStatusOverallCheck = 0,
BIMUploadRequestStatusLeadershipShift = 1,

};

extern NSString * const BimImageGuidName;

extern NSString * const BIMAmrGuidName;

@interface BIMBatchUploadRequest : BIMBaseRequest

+ (instancetype)requestStatus:(BIMUploadRequestStatus)status withImage:(NSArray *)imageDictionaryArray

withRecordFile:(NSArray*)amrDictionaryArray withParams:(NSMutableDictionary *)Params

delegate:(id<YTKRequestDelegate>)delegate;


@end





#import "BIMBatchUploadRequest.h"

#import "AFNetworking.h"

#import "HPAudioRecorderTool.h"

#import "LeaderPhotoFormCell.h"

#import "LeaderRecordFormCell.h"







NSString *const BimImageGuidName = @"{guid1}.jpg";




NSString *const BIMAmrGuidName = @"{guid2}.amr";







@interface BIMBatchUploadRequest ()




@property (nonatomic, assign)BIMUploadRequestStatus status;




@property (nonatomic, strong) NSArray  *imageDictionaryArray;

@property (nonatomic, strong) NSArray  *amrDictionaryArray;

@property (nonatomic, strong) NSMutableDictionary  *params;







@end




@implementation BIMBatchUploadRequest




+ (instancetype)requestStatus:(BIMUploadRequestStatus)status withImage:(NSArray *)imageDictionaryArray

withRecordFile:(NSArray*)amrDictionaryArray withParams:(NSMutableDictionary *)Params

delegate:(id<YTKRequestDelegate>)delegate {




//    BIMBatchUploadRequest *request = [[BIMBatchUploadRequest alloc] initWithParams:Params];



BIMBatchUploadRequest *request = [[BIMBatchUploadRequest alloc] init];

//    BIMBatchUploadRequest *request = [[BIMBatchUploadRequest alloc]initWithParams:Params];

request.params = Params;

request.status = status;

request.delegate = delegate;

request.imageDictionaryArray = imageDictionaryArray;

request.amrDictionaryArray = amrDictionaryArray;

//    request = (BIMBatchUploadRequest *)[request buildCustomUrlRequest];



[request start];

return request;

}




- (NSString *)requestUrl {




if (_status == BIMUploadRequestStatusOverallCheck) {



return @"api/App/CreateOverallCheck";

}

return @"api/App/CreateLeadershipShift";

}




/* url      :  本地文件路径

* name     :  与服务端约定的参数

* fileName :  自己随便命名的

* mimeType :  文件格式类型 [mp3 : application/octer-stream application/octet-stream] [mp4 : video/mp4]

*/




- (AFConstructingBlock)constructingBodyBlock {



return ^(id<AFMultipartFormData> formData) {



//        [self.paraDict enumerateKeysAndObjectsUsingBlock:^(id  _Nonnull key, id  _Nonnull obj, BOOL * _Nonnull stop) {

//

//            if ([obj isKindOfClass:[NSArray class]]) {

//

//                NSData *jsonData = [NSJSONSerialization dataWithJSONObject:obj options:NSJSONWritingPrettyPrinted error:nil];

//

//                [formData appendPartWithFormData:jsonData name:key];

//            }

//

//            if ([obj isKindOfClass:[NSString class]]) {

//

//                NSData *stringData = [obj dataUsingEncoding:NSUTF8StringEncoding];

//                [formData appendPartWithFormData:stringData name:key];

//            }

//        }];






//        for (NSInteger i = 0;i <[self.imageDictionaryArray count] ;i ++) {

//

//            NSDictionary *fileDictionary = self.imageDictionaryArray[i];

//

//            NSString *fileName = fileDictionary[photoImageNameTag];

//

//            UIImage *image  = fileDictionary[photoImageTag];

//            NSData *data    = UIImageJPEGRepresentation(image, 0.5);

//            NSString *name = BimImageGuidName;

//            NSString *type = @"image/jpeg/png/jpg";

//            [formData appendPartWithFileData:data name:name fileName:fileName mimeType:type];

//        }

};

}

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

[request setValue:[NSString stringWithFormat:@"%ld", body.length] forHTTPHeaderField:@"Content-Length"];

[request setValue:[BIMSQLiteManager manager].cookieToken forHTTPHeaderField:BIMRequestKeyCookie];

// 设置头部数据，指定了http post请求的编码方式为multipart/form-data（上传文件必须用这个）。

[request setValue:[NSString stringWithFormat:@"multipart/form-data; boundary=%@",BOUNDARY] forHTTPHeaderField:@"Content-Type"];




//token 放请求头

//    [request setValue:@"" forHTTPHeaderField:@"Authorization"];

return request;

}

- (YTKRequestMethod)requestMethod {

return YTKRequestMethodPOST;

}

- (BOOL)contentIsArray {

return false;

}
```
