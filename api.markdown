## 开启MAM


### startWithProductKey
启用MAM。user ID默认使用default_user

`+ (void)startWithProductKey:(NSString *)productKey`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| productKey | NSString * | 必填，产品标识符 |

#### 返回值

void

### setUserID：
动态设置用户ID，默认为default_user。传入nil或空字符串将不会有任何修改

`+ (void)setUserID:(NSString*)userID`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| userID | NSString * | 必填，设置userID |

#### 返回值

void

### startWithProductKey:userID:
启用MAM

`+ (void)startWithProductKey:(NSString *)productKey userID:(NSString *)userID`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| productKey | NSString * | 必填，产品标识符 |
| userID | NSString * | 选填，默认使用default_user |

#### 返回值

void

### startWithProductKey:delegate:
启用MAM。user ID默认使用default_user

`+ (void)startWithProductKey:(NSString *)productKey delegate:(id<MAMConfigDelegate>)delegate`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| productKey | NSString * | 必填，产品标识符 |
| delegate | `id<MAMConfigDelegate>` | 选填，使用MAMConfigDelegate协议配置更多的功能 |

#### 返回值

void

### startWithProductKey:userID:delegate:

启用MAM。user ID默认使用default_user

`+ (void)startWithProductKey:(NSString *)productKey userID:(NSString *)userID delegate:(id<MAMConfigDelegate>)delegate`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| productKey | NSString * | 必填，产品标识符 |
| userID | NSString * | 选填，默认使用default_user |
| delegate | `id<MAMConfigDelegate>` | 选填，使用MAMConfigDelegate协议配置更多的功能 |

#### 返回值

void

##使用网络诊断工具


### startNetDiagnoWithDomain:reason:completeBlock:
对指定域名执行Ping和Trace Route，运行DNS诊断工具，结束后返回诊断结果,并将结果上传到服务端

`+ (void)startNetDiagnoWithDomain:(NSString *)theDomain reason:(NSString *)reason completeBlock:(void ( ^ ) ( NSString *loginfo ))block`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| theDomain | NSString * | 必填，诊断域名 |
| reason | NSString * | 选填，诊断原因 |
| block | `void ( ^ ) ( NSString *loginfo )` | 选填，得到诊断结果 |

#### 返回值

void

### startNsInfoWithReason:completeBlock:

运行DNS检测工具（nstool.netease.com），结束后返回诊断结果,并将结果上传到服务端

`+ (void)startNsInfoWithReason:(NSString *)reason completeBlock:(void ( ^ ) ( NSString *loginfo ))block`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| reason | NSString * | 选填，诊断原因 |
| block | `void ( ^ ) ( NSString *loginfo )` | 选填，得到DNS诊断结果 |

#### 返回值

void

### startPingWithDomain:reason:completeBlock:

对指定域名执行Ping，结束后返回诊断结果,并将结果上传到服务端

`+ (void)startPingWithDomain:(NSString *)theDomain reason:(NSString *)reason completeBlock:(void ( ^ ) ( NSString *loginfo ))block`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| theDomain | NSString * | 必填，诊断域名 |
| reason | NSString * | 选填，诊断原因 |
| block | `void ( ^ ) ( NSString *loginfo )` | 选填，得到Ping诊断结果 |

#### 返回值

void

### startTraceRouteWithDomain:reason:completeBlock:

对指定域名执行Trace Route，结束后返回诊断结果

`+ (void)startTraceRouteWithDomain:(NSString *)theDomain reason:(NSString *)reason completeBlock:(void ( ^ ) ( NSString *loginfo ))block`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| theDomain | NSString * | 必填，诊断域名 |
| reason | NSString * | 选填，诊断原因 |
| block | `void ( ^ ) ( NSString *loginfo )` | 选填，得到Trace Route 诊断结果 |

#### 返回值

void

### setNeedCollectHeader:

设为YES时，对所有域名的请求都收集header； 设为NO时，不收集header收集规则参照+ setHeaderCollectRuleWithCollectHosts:unCollectHosts:

`+ (void)setNeedCollectHeader:(BOOL)needCollectHeader`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| needCollectHeader | BOOL | 设置header收集的全局开关 |

#### 返回值

void

### setHeaderCollectRuleWithCollectHosts:unCollectHosts:

设置header收集规则。  
setNeedCollectHeader 为YES时，根据header name判断是否收集header；  
setNeedCollectHeader 为NO时：  
如果host包含在collectHeaderNames，根据header name判断是否收集header，  
如果host包含在unCollectHosts中，不收集header，  
如果collectHosts和unCollectHosts都不包含该请求的host，根据header name判断是否收集header

`+ (void)setHeaderCollectRuleWithCollectHosts:(NSArray *)collectHosts unCollectHosts:(NSArray *)unCollectHosts collectHeaderNames:(NSArray <NSString*> *)collectHeaderNames`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| collectHosts | NSArray * | 选填，收集header的host列表 |
| unCollectHosts | NSArray * | 选填，不收集header的host列表 |
| collectHeaderNames | NSArray * | 选填，只收集特定header名 |

#### 返回值

void

### postSynchronousDataWithHeader:dataContents:error:

使用MAM的发送处理完成数据向MAM后台的上传。使用了CFNetwork，防止了NSURLProtocol的拦截

`+ (NSData*)postSynchronousDataWithHeader:(NSDictionary <NSString * , NSString *> *)header
                            dataContents:(NSArray <NSDictionary *> *)dataContents
                                   error:(NSError **)error`

#### 参数
|参数|类型|描述|
| -- | -- | -- |
| header | NSDictionary <NSString * , NSString *> * | 必填，MAM后台需要的header数据 |
| dataContents | NSArray <NSDictionary *> * | 必填，上传的数据内容 |
| error | NSError ** | 选填，error |

#### 返回值
|类型|描述|
| -- | -- |
| NSData* | MAM后台接收到上传数据后返回的数据 |


### setTraceEnable:

动态关闭和打开MAM的监控功能，只能在使用方法startWithProductKey:开启MAM之后生效

`+ (void)setTraceEnable:(BOOL)isEnabled`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| isEnabled | BOOL | 是否启用MAM |

#### 返回值

void

### isTraceEnable

MAM监控功能是否打开

`+ (BOOL)isTraceEnable`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | MAM监控功能是否打开 |

### setSamplingRate:

网络请求数据上传的采样率，不影响异常网络请求的上传

`+ (void)setSamplingRate:(int)percent;`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| percent | int | 正常网络请求上传采样率的百分比 |

#### 返回值

void

##	MAMConfigDelegate协议


### MAMUploadBatchSize
设置网络数据上传的批量大小，当未发送的数据个数达到批量限制时，触发发送动作 默认值 5

`- (int)MAMUploadBatchSize`

#### 返回值

|类型|描述|
| -- | -- |
| int | 用于设置上传批量大小，默认值为5 |

### MAMUploadInterval
设置批量上传模式下，数据上传最大时间间隔,以秒为单位 存在可发送数据但是又没有满足上传批量时，如果发送间隔超过这个数值，将触发发送动作 默认值 60

`- (int)MAMUploadInterval`

#### 返回值

|类型|描述|
| -- | -- |
| int | 用于设置最大发送间隔，默认值为60 |

### MAMPostEnableNetworkEnvironment

设置可发送数据的最低网络环境 默认返回值 MAMPostEnableAfter2G，所有网络环境下都发送数据，即在2G、3G、4G和Wifi环境下可发送数据

`- (MAMPostEnableNetwork)MAMPostEnableNetworkEnvironment`

#### 返回值

|类型|描述|
| -- | -- |
| MAMPostEnableNetwork | 用于设置允许发送数据的最低网络环境 |

### MAMCacheDirectoryPath

设置本地缓存的数据库位置，当前网络环境比允许的最低发送网络还差时，数据需要缓存在本地数据库。 MAM会在这个路径上创建一个MAMDataFile文件夹，包含一个文件NAPMDataFile.sql。

`- (NSString *)MAMCacheDirectoryPath`

#### 返回值

|类型|描述|
| -- | -- |
| NSString * | 设置本地数据库位置。返回路径不可用时使用默认路径，默认使用Cache文件夹。 |

### MAMNeedMonitorNetTraffic

监测网络数据的开关，用于便捷地关闭网络数据监控，默认开启  

`- (BOOL)MAMNeedMonitorNetTraffic`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | 是否开启监控NSURLConnection，CFNetwork，UIWebView的网络数据 |

### MAMNeedMonitorNetTrafficNSURLConnection

监测NSURLConnection网络数据的开关，默认开启。注意单独关闭该方法时，如果MAMNeedMonitorNetTrafficWebViewUseURLProtocol方法返回YES的情况下，由于URLProtocol的原因，依然会收集到NSURLConnection的数据。所以如果有需要关闭对NSURLConnection的监控时，需要同时关闭MAMNeedMonitorNetTrafficWebViewUseURLProtocol；  

`- (BOOL)MAMNeedMonitorNetTrafficNSURLConnection`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | 是否监控NSURLConnection |

### MAMNeedMonitorNetTrafficCFNetwork

监测CFNetwork网络数据的开关，默认开启

`- (BOOL)MAMNeedMonitorNetTrafficCFNetwork`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | 是否监控CFNetwork |

### MAMNeedMonitorNetTrafficNSURLSession

监测NSURLSession网络数据的开关，默认关闭

`- (BOOL)MAMNeedMonitorNetTrafficNSURLSession`

#### 返回值

| 类型 | 描述 |
| ------ | ------ | 
| BOOL | 是否监控NSURLSession |


### MAMNeedMonitorNetTrafficWebViewUseURLProtocol
使用URLProtocol监控的开关，默认开启。当MAMNeedMonitorNetTrafficNSURLConnection返回YES时，使用URLProtocol监控WebView和重定向后的NSURLConnection的请求；当返回NO时，会监控到所有的NSURLConnection请求和WebView的请求。注意URLProtocol不仅能监控UIWebView的网络请求，还可以监控系统执行重定向后的NSURLConnection的请求。 

`- (BOOL)MAMNeedMonitorNetTrafficWebViewUseURLProtocol`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | 是否监控WebView和NSURLConnection重定向数据 |

### MAMNeedUIPerformanceTrace

启用页面性能检测，默认关闭。监控页面的viewDidLoad，viewWillLayoutSubviews，viewDidLayoutSubviews，viewWillAppear，viewDidAppear，viewDidDisappear等方法的时间戳。

`- (BOOL)MAMNeedUIPerformanceTrace`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | 是否启用页面性能检测 |

### MAMPostDataCatchWithHeader: dataContents:

代替MAM的数据上传，取得需要发送到MAM后台的数据。该方法在后台线程中执行。

`- (void)MAMPostDataCatchWithHeader:(NSDictionary <NSString * , NSString *> *)header
                      dataContents:(NSArray <NSDictionary *> *)dataContents`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| header | `NSDictionary <NSString * , NSString *> *` | 上传到MAM后台的http请求需要的header内容。"X-NAPM-MarkDataUpload"字段标记了请求是MAM采集数据的上传，将绕过MAM的捕获系统 |
| dataContents | `NSArray <NSDictionary *> *` | dataContents 上传数据的内容。用户自己发送时，需要将dataContents转换成JSON格式Data，添加到HTTP Post请求上。用户自己发送数据时，如果对JSON data做了gzip压缩，需要将 header 中@"X-MAM-Compressed"字段对应的值从@"flase"修改为@"true"；使用+[MAM postSynchronousDataWithHeader: dataContents: error:]发送时不需要。 |

#### 返回值

void
