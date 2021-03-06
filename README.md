# iOS-ToolClassDemo
###开发工具类(可以直接取出工程目录***[tool](https://github.com/VolientDuan/iOS-ToolClassDemo/tree/master/testToolDemo/testToolDemo/tool)***文件下的类使用)
#####宏定义的使用

######一、pch文件中宏的使用

1.屏幕的宽度和高度

	#define SCREEN_WIDTH   [[UIScreen mainScreen] bounds].size.width
	#define SCREEN_HEIGHT  [[UIScreen mainScreen] bounds].size.height

2.16进制颜色转换

	#define UIColorFromRGB(rgbValue) [UIColor colorWithRed:((float)((rgbValue & 0xFF0000) >> 16))/255.0 green:((float)((rgbValue & 0xFF00) >> 8))/255.0 blue:((float)(rgbValue & 0xFF))/255.0 alpha:1.0]
3.自定义log

	#define DEBUGGER 1 //上线版本屏蔽此宏
	
	#ifdef DEBUGGER
	/* 自定义log 可以输出所在的类名,方法名,位置(行数)*/
	#define VDLog(format, ...) NSLog((@"%s [Line %d] " format), __FUNCTION__, __LINE__, ##__VA_ARGS__)
	
	#else
	
	#define VDLog(...)
	
	#endif


#####目前包含一下工具类：
######一、NSString的扩展类

1.NSString+tool

* 空判断
* 获取DeviceIdentifierForVendor
* 时间戳转化为字符串
* 富文本转化

######二、UIView的扩展类

1.UIView+tool

* 获取frame属性和单个属性的设置
* 设置圆角半径

2.UIView+EmptyShow

	/**
	 *  空页面显示提醒图与文字并添加重新刷新
	 *
	 *  @param emptyType 页面的展示的数据类别（例如：我的订单或者web页）
	 *  @param haveData  是否有数据
	 *  @param block     重新加载页面（不需要时赋空）
	 */
	- (void)emptyDataCheckWithType:(ViewDataType)emptyType
	                   andHaveData:(BOOL)haveData
	                andReloadBlock:(ReloadDataBlock)block;
示例图：

![emptyOrder.png](https://github.com/VolientDuan/iOS-ToolClassDemo/blob/master/md+image/emptyOrder.png?raw=true)	                
######三、UIImage的扩展类

1.UIImage+tool：对image的尺寸和大小进行修改以及截图

#####四、UIControl的扩展类
1.UIControl+clickRepeatedly（处理按钮的反复点击）

不用再纠结此类问题了，一句代码搞定：

	btn.clickInterval = 3;
#####五、用户信息（单例模式）
UserInfoModel:实现一些轻量级的用户信息存储

#####六、AFNetWorking的二次封装（单例模式）
1.简单的HTTP（POST）请求：其中添加了多种请求错误判断和debug模式下打印响应成功的数据，最后采用block进行响应结果的回调

	/**
	 *  发送请求
	 *
	 *  @param requestAPI 请求的API
	 *  @param vc         发送请求的视图控制器
	 *  @param params     请求参数
	 *  @param className  数据模型
	 *  @param response   请求的返回结果回调
	 */
	- (void)sendRequestWithAPI:(NSString *)requestAPI
	                    withVC:(UIViewController *)vc
	                withParams:(NSDictionary *)params
	                 withClass:(Class)className
	             responseBlock:(RequestResponse)response;

2.创建下载任务并对下载进度，任务对象和存储路径等进行回调（支持多任务下载），创建的任务会自动加到下载队列中，下载进度的回调自动回到主线程（便于相关UI的操作）

	/**
	 *  创建下载任务
	 *
	 *  @param url          下载地址
	 *  @param fileName     文件名
	 *  @param downloadTask 任务
	 *  @param progress     进度
	 *  @param result       结果
	 */
	- (void)createDdownloadTaskWithURL:(NSString *)url
	              withFileName:(NSString *)fileName
	                       Task:(DownloadTask)downloadTask
	                   Progress:(TaskProgress)progress
	                     Result:(TaskResult)result;

3.创建上传任务与下载方法回调基本一致

	/**
	 *  创建上传任务
	 *
	 *  @param url        上传地址
	 *  @param mark       任务标识
	 *  @param data       序列化文件
	 *  @param uploadTask 任务
	 *  @param progress   进度
	 *  @param result     结果
	 */
	- (void)createUploadTaskWithUrl:(NSString *)url
	                       WithMark:(NSString *)mark
	                       withData:(NSData *)data
	                           Task:(UploadTask)uploadTask
	                       Progress:(TaskProgress)progress
	                         Result:(TaskResult)result;
                         
#####七、校验工具类
1.手机号校验

	+(BOOL) isValidateMobile:(NSString *)mobile
2.邮箱校验

	+(BOOL)isValidateEmail:(NSString *)email
3.车牌号校验

	+(BOOL) isvalidateCarNo:(NSString*)carNo
4.身份证号验证

	+(BOOL) isValidateIDCardNo:(NSString *)idCardNo