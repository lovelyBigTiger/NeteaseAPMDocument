## 开启MAM


### startWithProductKey:
开启MAM，用户ID默认为default_user

`+ (void)startWithProductKey:(NSString *)productKey`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| productKey | NSString * | 产品ID，必填 |

#### 返回值

void

### setUserID：
设置用户ID

`+ (void)setUserID:(NSString*)userID`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| userID | NSString * | 用户ID，默认为default_user |

#### 返回值

void

### setDeviceID:
设置 device ID，默认为 `-[UIDevice identifierForVendor]`

`+ (void)setDeviceID:(NSString*)deviceID`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| deviceID | NSString * | device ID |

#### 返回值

void

### startWithProductKey:userID:
开启MAM

`+ (void)startWithProductKey:(NSString *)productKey userID:(NSString *)userID`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| productKey | NSString * | 产品ID，必填 |
| userID | NSString * | 用户ID |

#### 返回值

void

### startWithProductKey:delegate:
开启MAM，用户ID默认为default_user

`+ (void)startWithProductKey:(NSString *)productKey delegate:(id<MAMConfigDelegate>)delegate`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| productKey | NSString * | 产品ID，必填 |
| delegate | `id<MAMConfigDelegate>` | MAMConfigDelegate 详细配置 |

#### 返回值

void

### startWithProductKey:userID:delegate:

开启MAM

`+ (void)startWithProductKey:(NSString *)productKey userID:(NSString *)userID delegate:(id<MAMConfigDelegate>)delegate`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| productKey | NSString * | 产品ID，必填 |
| userID | NSString * | 用户ID |
| delegate | `id<MAMConfigDelegate>` | MAMConfigDelegate 详细配置 |

#### 返回值

void

## 使用网络诊断工具


### startNetDiagnoWithDomain:reason:completeNeedUploadBlock:
执行Ping，Trace Route，和DNS诊断工具

`+ (void)startNetDiagnoWithDomain:(NSString *)theDomain reason:(NSString *)reason completeNeedUploadBlock:(BOOL(^)(NSString *loginfo))block`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| theDomain | NSString * | 诊断域名，必填 |
| reason | NSString * | 诊断原因 |
| block | `BOOL(^)(NSString *loginfo)` | 得到诊断结果，返回YES数据上传到NAPM后台 |

#### 返回值

void

### startNsInfoWithReason:completeNeedUploadBlock:

运行DNS检测工具（nstool.netease.com），结束后返回诊断结果,并将结果上传到服务端

`+ (void)startNsInfoWithReason:(NSString *)reason completeNeedUploadBlock:(BOOL(^)(NSString *loginfo))block`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| reason | NSString * | 诊断原因 |
| block | `BOOL(^)(NSString *loginfo)` | 得到诊断结果，返回YES数据上传到NAPM后台 |

#### 返回值

void

### startPingWithDomain:reason:completeBlock:

对指定域名执行Ping，结束后返回诊断结果,并将结果上传到服务端

`+ (void)startPingWithDomain:(NSString *)theDomain reason:(NSString *)reason completeNeedUploadBlock:(BOOL(^)(NSString *loginfo))block`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| theDomain | NSString * | 诊断域名，必填 |
| reason | NSString * | 诊断原因 |
| block | `BOOL(^)(NSString *loginfo)` | 得到诊断结果，返回YES数据上传到NAPM后台 |

#### 返回值

void

### startTraceRouteWithDomain:reason:completeBlock:

对指定域名执行Trace Route，结束后返回诊断结果

`+ (void)startTraceRouteWithDomain:(NSString *)theDomain reason:(NSString *)reason completeNeedUploadBlock:(BOOL(^)(NSString *loginfo))block`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| theDomain | NSString * | 诊断域名，必填 |
| reason | NSString * | 诊断原因 |
| block | `BOOL(^)(NSString *loginfo)` | 得到诊断结果，返回YES数据上传到NAPM后台 |

#### 返回值

void

### setNeedCollectHeader:

配置是否收集正常网络请求（无网络错误且 state code 为2XX）收集 header，默认为NO，不收集。使用`setHeaderCollectRule`指定详细收集规则

`+ (void)setNeedCollectHeader:(BOOL)needCollectHeader`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| needCollectHeader | BOOL | 是否收集正常网络请求收集 header |

#### 返回值

void

### setHeaderCollectRuleWithCollectHosts:unCollectHosts:

设置 header 收集规则，`needCollectHeade` 为NO时仍然生效

`+ (void)setHeaderCollectRuleWithCollectHosts:(NSArray *)collectHosts unCollectHosts:(NSArray *)unCollectHosts collectHeaderNames:(NSArray <NSString*> *)collectHeaderNames`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| collectHosts | NSArray <NSString*> * | 指定收集的host列表，默认nil，全收集 |
| unCollectHosts | NSArray <NSString*> * | 指定不收集的host列表，默认nil，全收集 |
| collectHeaderNames | NSArray <NSString*> * | 指定强制收集header列表，默认nil，全收集 |

#### 返回值

void

### setTraceEnable:

在开启MAM之后，开关MAM收集数据功能

`+ (void)setTraceEnable:(BOOL)isEnabled`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| isEnabled | BOOL | 是否开启MAM收集数据功能 |

#### 返回值

void

### isTraceEnable

MAM收集数据功能是否开启，默认YES

`+ (BOOL)isTraceEnable`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | MAM收集数据功能是否开启 |

### setSamplingRate:

设置正常网络请求（无网络错误且 state code 为2XX）数据收集的默认采样率，采样率 = percent/100.0

`+ (void)setSamplingRate:(int)percent;`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| percent | int | 默认采样率 |

#### 返回值

void

### setSamplingRateWithDefaultPercent:percentsOnHost:

设置正常网络请求（无网络错误且 state code 为2XX）数据收集的默认采样率，并设置指定host的采样率，采样率 = percent/100.0

`+ (void)setSamplingRate:(int)percent;`

#### 参数

|参数|类型|描述|
| -- | -- | -- |
| defaultPercent | int | 默认采样率 |
| hostPercentDic | NSDictionary <NSString *, NSNumber *>* | 指定host的采样率，默认nil，使用默认采样率 |

#### 返回值

void

## MAMConfigDelegate协议


### MAMUploadBatchSize
设置网络监控数据一次上传最大数据条数，默认为5

`- (int)MAMUploadBatchSize`

#### 返回值

|类型|描述|
| -- | -- |
| int | 网络监控数据一次上传最大数据条数 |

### MAMUploadInterval
设置网络监控数据条数未满`MAMUploadBatchSize`时的最大等待时间，默认为60

`- (int)MAMUploadInterval`

#### 返回值

|类型|描述|
| -- | -- |
| int | 网络监控数据条数未满`MAMUploadBatchSize`时的最大等待时间 |

### MAMPostEnableNetworkEnvironment

设置MAM上传监控数据的最差网络环境，默认为2G

`- (MAMPostEnableNetwork)MAMPostEnableNetworkEnvironment`

#### 返回值

|类型|描述|
| -- | -- |
| MAMPostEnableNetwork | 允许MAM上传监控数据的最差网络环境 |

### MAMCacheDirectoryPath

MAM缓存数据位置，默认为Documents文件夹

`- (NSString *)MAMCacheDirectoryPath`

#### 返回值

|类型|描述|
| -- | -- |
| NSString * | MAM缓存数据位置 |

### MAMNeedMonitorNetTraffic

设置是否监控网络请求数据，默认YES

`- (BOOL)MAMNeedMonitorNetTraffic`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | 是否监控网络请求数据 |

### MAMNeedMonitorNetTrafficNSURLSession

设置是否监测NSURLSession发生的网络请求，默认NO

`- (BOOL)MAMNeedMonitorNetTrafficNSURLSession`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | 是否监测NSURLSession发生的网络请求 |

### MAMNeedMonitorNetTrafficNSURLConnection

设置是否监测NSURLConnection发生的网络请求，默认YES  

`- (BOOL)MAMNeedMonitorNetTrafficNSURLConnection`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | 是否监测NSURLConnection发生的网络请求n |

### MAMNeedMonitorNetTrafficCFNetwork

设置是否监测CFNetwork发生的网络请求，默认NO

`- (BOOL)MAMNeedMonitorNetTrafficCFNetwork`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | 是否监测CFNetwork发生的网络请求 |

### MAMEnableNetworkLowLevelRead

打开网络较底层的监控功能，默认NO  
支持获取iOS10 64位 底层数据：DNS Server IP，DNS 结果，DNS 耗时，连接耗时，Server IP等数据

`- (BOOL)MAMNeedMonitorNetTrafficCFNetwork`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | 是否读取底层网络数据 |

### MAMNeedMonitorNetTrafficWebViewUseURLProtocol
使用`MAMEnableNetworkLowLevelRead`时需要设置是否在模拟器上运行，默认NO 
```
-(BOOL)MAMisUsingSimulator
 {
 #if TARGET_IPHONE_SIMULATOR
 return YES;
 #else
 return NO;
 #endif
 }
 ```
 
`- (BOOL)MAMisUsingSimulator`

#### 返回值

|类型|描述|
| -- | -- |
| BOOL | 是否使用模拟器 |

