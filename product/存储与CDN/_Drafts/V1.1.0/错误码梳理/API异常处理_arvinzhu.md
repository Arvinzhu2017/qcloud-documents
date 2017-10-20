### ActionS3FileGet

1. 当下载私有读权限的文件时，如果请求携带签名不正确，则返回403，错误码为AccessDenied，用户可以排查签名算法是否正确
2. 如果请求携带x-cos-server-side-encryption头域，则返回400，错误码为SSEHeaderNotAllowed，下载请求中不允许携带x-cos-server-side-encryption头域
3. 如果携带Range头部进行下载，当Range头域格式不正确时，将返回整个文件数据；当Range范围超过与文件大小没有交集时，则返回416，错误码InvalidRange
4. 如果请求携带If-Modified-Since. If-Unmodified-Since头部格式非法，则返回400，错误码为InvalidArgument
5. 如果请求携带If-Unmodified-Since，当文件修改时间早于或等于指定时间，才返回文件内容，否则返回412，错误码PreconditionFailed
6. 如果请求携带If-Modified-Since，当文件在指定时间后被修改，则返回对应Object meta信息，否则返回 304
7. 如果请求携带If-Match的内容与文件ETAG不一致，则返回412，错误码PreconditionFailed
8. 如果请求携带If-None-Match的内容与文件ETAG不一致，才返回文件，否则返回 304
9. 如果下载文件不存在，则返回404 Not Found错误，错误码NoSuchKey
10. 如果下载文件指定的Bucket不存在，则返回404 Not Found错误，错误码NoSuchBucket
11. 如果待下载文件已经沉降到CAS，且COS中也没有通过Restore请求恢复可用副本，则下载会返回403，错误码为InvalidObjectState

### ActionS3FileInitMultiPartUpload

1. 当向私有写权限的bucket上传文件时，如果请求携带签名不正确，则返回403，错误码为AccessDenied，用户可以排查签名算法是否正确
2. 如果请求携带x-cos-server-side-encryption头域且其值不为AES256，则返回400，错误码为SSEContentNotSupported，COS不允许非AES256模式的服务端加密

### ActionS3FileUploadPart

1. 当向私有写权限的bucket上传文件时，如果请求携带签名不正确，则返回403，错误码为AccessDenied，用户可以排查签名算法是否正确
2. 如果请求携带的uploadId无效，则返回404，错误码为NoSuchUpload
3. 如果请求没有携带Content-Length头部，则返回411，错误码为MissingContentLength
4. 如果请求携带Content-Length头部的值超过5GB，则返回400，错误码为EntityTooLarge
5. 如果请求没有携带partNumber参数或者该参数范围不在1~10000范围内，则返回400，错误码为InvalidArgument

### ActionS3FileUploadPartComplete

1. 当向私有写权限的bucket上传文件时，如果请求携带签名不正确，则返回403，错误码为AccessDenied，用户可以排查签名算法是否正确
2. 如果请求携带的uploadId无效，则返回404，错误码为NoSuchUpload
3. 如果请求携带的xml消息体格式不规范，则返回400，错误码为MalformedXML
4. 当上传块小于 1 MB 的时候，在调用该 API 时，会返回 400 EntityTooSmall
5. 当上传块编号不连续的时候，在调用该 API 时，会返回 400 InvalidPart
6. 当请求 Body 中的块信息没有按序号从小到大排列的时候，在调用该 API 时，会返回 400 InvalidPartOrder

### ActionS3FileListParts

1. 如果文件是私有读权限，当请求携带签名不正确时，则返回403，错误码为AccessDenied，用户可以排查签名算法是否正确
2. 如果请求携带的uploadId无效，则返回404，错误码为NoSuchUpload
3. 如果请求携带的max-parts参数不在范围1~1000内，则返回400， 错误码为InvalidArgument
4. 如果请求携带的参数encoding-type不为“url”，则返回400，错误码为InvalidArgument

### ActionS3FileUploadPartAbort

1. 如果文件是私有写权限，当请求携带签名不正确时，则返回403，错误码为AccessDenied，用户可以排查签名算法是否正确
2. 如果请求携带的uploadId无效，则返回404，错误码为NoSuchUpload

### ActionS3PostObjRestore

1. 如果文件是私有读权限，当请求携带签名不正确时，则返回403，错误码为AccessDenied，用户可以排查签名算法是否正确
2. 如果请求恢复的文件不存在，则返回404，错误码为NoSuchKey
3. 如果请求恢复的文件还没有沉降到CAS中，则返回405，错误码为RestoreNonArchiveObject
4. 如果请求恢复的文件正在从CAS恢复到COS中，恢复还未完成，则返回409，错误码RestoreAlreadyInProgress
