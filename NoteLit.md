******Guid**

* [1.App版本号](#1)
* [2.网址的编码解码](#2)
* [3.通过UIView找Controller](#3)
* [4.关于URL的属性](#4)
* [5.字符串删除首尾空格和换行](#5)
* [6.绘制纯色image](#6)
* [7.Xcode7联网](#7)
* [8.xib设置颜色](#8)
* [9.查找工程中没有用到的文件--fui](#9)
* [10.自定义导航条背景色（图片）](#10)
* [11.自定义导航条标题属性](#11)
* [12.tabbar自定义图片 （更改图片的渲染模式）](#12)
* [13.版本是适配问题](#13)
* [14.工程中出现了如下警告](#14)
* [15.Developer文件夹](#15)
* [16.关于沙盒路径](#16)

<h5 id="1">1.App版本号</h5>
	
	[[[NSBundle mainBundle] infoDictionary] valueForKey:@"CFBundleVersion"]`

	[[[NSBundle mainBundle] infoDictionary] objectForKey:@"CFBundleShortVersionString"]
	
<h5 id="2">2.网址的编码解码</h5>


	[urlStr stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];

	[urlStr stringByReplacingPercentEscapesUsingEncoding:NSUTF8StringEncoding];


<h5 id="3">3.通过UIView找Controller</h5>

	+ (UINavigationController *)findNavigationControllerForView:(UIView *)view
	{
		UIResponder *resp = [view nextResponder];		
   		while (resp && ![resp isKindOfClass:[UINavigationController class]] 
    {
        resp = [resp nextResponder];
    }
    	return (UINavigationController *)resp;
	}
	
<h5 id="4">4.关于URL的属性</h5>

	@property (readonly, copy) NSString *absoluteString;
	@property (readonly, copy) NSString *relativeString;
	@property (readonly, copy) NSURL *baseURL; 
	@property (readonly, copy) NSURL *absoluteURL; 
	@property (readonly, copy) NSString *scheme;
	@property (readonly, copy) NSString *resourceSpecifier;
	@property (readonly, copy) NSString *host;
	@property (readonly, copy) NSNumber *port;
	@property (readonly, copy) NSString *user;
	@property (readonly, copy) NSString *password;
	@property (readonly, copy) NSString *path;
	@property (readonly, copy) NSString *fragment;
	@property (readonly, copy) NSString *parameterString;
	@property (readonly, copy) NSString *query;
	@property (readonly, copy) NSString *relativePath;
	
<h5 id="5">5.字符串删除首尾空格和换行</h5>


	[textView.text stringByTrimmingCharactersInSet:[NSCharacterSet whitespaceAndNewlineCharacterSet]];
	/Users/apple/WHotelForIOS/Classes/ResultTableViewCell.m
<h5 id="6">6.绘制纯色image</h5>

	- (UIImage *)imageWithColor:(UIColor *)color {
	
    CGRect rect = CGRectMake(0.0f, 0.0f, 1.0f, 1.0f);
    UIGraphicsBeginImageContext(rect.size);
    CGContextRef context = UIGraphicsGetCurrentContext();
    
    CGContextSetFillColorWithColor(context, [color CGColor]);
    CGContextFillRect(context, rect);
    
    UIImage *image = UIGraphicsGetImageFromCurrentImageContext();
    UIGraphicsEndImageContext();
    
    return image;
}

<h5 id="7">7.Xcode7联网</h5>
	
![](http://images.cnblogs.com/cnblogs_com/isfun/716972/o_xcode7https.jpg)
	
<h5 id="8">8.xib设置颜色</h5>
	
![](http://images.cnblogs.com/cnblogs_com/isfun/716972/o_Xcodecolor.png)

<h5 id="9">9.查找工程中没有用到的文件--fui</h5>

	· 安装fui ：Sudo gem install fui 
	· 查找僵尸文件：fui find
	· 帮助： fui -help


<h5 id="10">10.自定义导航条背景色（图片）</h5>

	[[UINavigationBar appearance] setBackgroundImage:image forBarMetrics:...];

<h5 id="11">11.自定义导航条标题属性</h5>

	[nav.navigationBar setTitleTextAttributes:attributes];

<h5 id="12">12.tabbar自定义图片 （更改图片的渲染模式）</h5>
 
	nav.tabBarItem.image = image;
	nav.tabBarItem.image = [image imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];


<h5 id="13">13.版本是适配问题</h5>
	
	例如，对于iOS7的SDK，会多一个宏定义：
	#define __IPHONE_7_0     60100  
	
	所以在项目中，要想对相应的版本进行兼容，只需要判断是否有该宏定义即可。
	
	#ifdef __IPHONE_7_0  
        self.window.tintColor = [UIColor redColor];  
    #endif  
    
<h5 id="14">14.工程中出现了如下警告</h5>

`Attribute Unavailable: Automatic Preferred Max Layout Width is not available on iOS versions prior to 8.0`

	解决方法：找到设置label的preferred width，勾选explicit就行。
	但是，自动布局可能会出现一些问题。至于preferred width有什么用，现在还不清楚

<h5 id="16">15.Developer文件夹</h5>

·`Xcode->Products`

	 有项目的crash日志。猜测是AppStore同步下来的。如果真的是这样。真是碉堡了~

·`Xcode->Archives` 

	项目打包的都在这儿

·`Xcode->DeriveData` 

	编译产生的缓存文件都藏匿在这。隔一段时间干掉它
	
·`Xcode->CoreSimulator`

	所有的模拟器数据都在这。
	
	沙盒路径：device->simulater->containers->data->applications->程序数据->动态沙盒路径（IOS8以后）
	
<h5 id="16">16.关于沙盒路径</h5>
	
	ios8以后，沙盒路径编程动态的；
	Documents：应用中用户数据可以放在这里，iTunes备份和恢复的时候会包括此目录
	tmp：存放临时文件，iTunes不会备份和恢复此目录，此目录下文件可能会在应用退出后删除
	Library/Caches：存放缓存文件，iTunes不会备份此目录，此目录下文件不会在应用退出删除

<h5 id="17">17.storyboard ID & restoreation ID</h5>
	
	通过sotryboardID来找到 在sotryboard里面的viewcontroller --> so cool!
	UIStoryboard* mainStoryboard = [UIStoryboard storyboardWithName:@"MainStoryboard" bundle:nil];
	ViewController *controller = [mainStoryboard instantiateViewControllerWithIdentifier:@"some story id"];
	
	restoration ID  恢复标识。应用程序退出并且终止后再次进来的时候，为了实现UI状态保持和恢复添加的设置项目。 （目前  并没什么卵用）
	
	
<h5 id="18">18.IBOutletCollection(UIButton)</h5>

	@property (nonatomic) IBOutletCollection(UIButton) NSArray *buttons; --> 可以将ib文件中的buttons作为数组Outlet到工程中;
	
<h5 id="19">19.关于UIStoryboardSegue</h5>
	
	使用storyboard构建controllers时候，中间传值问题，需要Segue这一神器来manage.
	
20.@try异常处理

	@try {
		异常处理可能事件
  	 }
  	 @catch (NSException *exception) {
  	 	如果try出现异常，执行此处的事件。如果仍为异常事件，则crash；
  	 	否则不执行。
  	 }
  	 @finally {
		一定会执行 但是也只是打印日志什么的  如否上面两个事件都异常，一样会crash
  	 }

21.关于storBoard的代理的设置

22.textField 设置placeHolder字体的属性

	textfiled.attributedPlaceholder  <- 属性字符串
	
23.`setvalue forkey` VS `setObject forKey`

	1, setObject：forkey：中value是不能够为nil的，不然会报错。setValue：forKey：中value能够为nil，但是当value为nil的时候，会自动调用removeObject：forKey方法 []
	2, setValue：forKey：中key的参数只能够是NSString类型，而setObject：forKey：的可以是任何类型
	
24.关于winsize
	
	#define kWinSize [[UIApplication sharedApplication] keyWindow].bounds.size
	这个宏定义的前提是UIApplication已经加载完成，所以有时候 这个macro可能不会生效
	一般用UISCreen来宏定义winsize
	
25.extern 、constant 、 static

	
26.

	UIStoryboardSegue 的 perform方法
	willMoveToParentViewController -> didMoveToParentViewController
	controller: removeFromParentViewController
	view: removeFromSuperview
	
	
	
27.[self.view layoutIfNeeded];
	
	要想在代码里使用ib文件的view的尺寸 一般情况下期望得到的时layout之后的尺寸      [self.view layoutIfNeeded];  是个好东西~~~
	
	
28.关于sear display controller的tableView
	

![](http://images.cnblogs.com/cnblogs_com/isfun/716972/o_lll.gif) 

![](http://images.cnblogs.com/cnblogs_com/isfun/716972/o_lllss.gif)
	
	
	- (void)searchDisplayController:(UISearchDisplayController *)controller willShowSearchResultsTableView:(UITableView *)tableView {
    [tableView setContentInset:UIEdgeInsetsZero];
    [tableView setScrollIndicatorInsets:UIEdgeInsetsZero];
	}
	
	只需回调一下这个代理方法就行 √
	
	
	
	
29.init操作中不能进行UI操作,didLoad之后再干UI的事儿


30.ib中scrollView 的自动布局

[参考](http://www.cocoachina.com/ios/20150104/10810.html)


31.UISearchDisplayController “无结果”文字的自定义

	 for(UIView *subview in self.searchDisplay.searchResultsTableView.subviews) {
		 if([subview isKindOfClass:[UILabel class]]) {
		 	[(UILabel*)subview setText:@"XXXXXXX"];
		}
	}

32.多线程--dispitch队列

* 线程队列	

		串行队列：dispatch_queue_t queue = dispatch_queue_create ( "com.dispatch.serial" , DISPATCH_QUEUE_SERIAL ); 
		->队列中的block按照先进先出（FIFO）的顺序去执行，实际上为单线程执行。

		并行队列：dispatch_queue_t  queue =  dispatch_queue_create ( "com.dispatch.concurrent" ,  DISPATCH_QUEUE_CONCURRENT );
		->生成一个并发执行队列，block被分发到多个线程去执行
	
		程序进程缺省产生的并发队列:dispatch_queue_t  queue =  dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0); 
	
		主线程：dispatch_queue_t  queue =  dispatch_get_main_queue(); 
		获得主线程的dispatch队列，实际是一个串行队列
* 同步&异步

		dispatch_async(queue, ^{}); 
		dispatch_async(queue, ^{}); 
		
* 应用举例

		常见的网络请求数据多线程执行模型：

		dispatch_async(dispatch_get_global_queue(DISPATCH_QUEUE_PRIORITY_DEFAULT, 0), ^{
		//子线程中开始网络请求数据
		//更新数据模型
			dispatch_sync(dispatch_get_main_queue(), ^{
			//在主线程中更新UI代码
			});
		});



33.设置圆角后，滑动卡顿的现象。

	原因不解释。
	加爵方法：缓存。
	self.layer.shouldRasterize = YES;  
	self.layer.rasterizationScale = [UIScreen mainScreen].scale;  

		
34.searchDisplayController的黑边问题

	设置一下nav.view的background就行
	
    [self.searchDisplayController.searchBar setBackgroundImage:[UIImage imageWithCGImage:(__bridge CGImageRef)(kBlueColor)]];//searchBar 背景班头颜色透明剞劂方案
    self.navigationController.view.backgroundColor = kBlueColor;//开始搜索时出现的黑色

36.常用测试打印
	
	比如 打印方法名：   
	NSLog(@"%s",__FUNCTION__);
    NSLog(@"%@",NSStringFromSelector(_cmd))
    
    项目应用示范:
    #ifdef DEBUG
	#define debugLog(...) NSLog(__VA_ARGS__)
	#define debugMethod() NSLog(@"%s", __func__)
	#else
	#define debugLog(...)
	#define debugMethod()
	#endif




37.忽略警告
	
	1.方法弃用告警
	#pragma clang diagnostic push  
  	#pragma clang diagnostic ignored "-Wdeprecated-declarations"       
	[TestFlight setDeviceIdentifier:[[UIDevice currentDevice] uniqueIdentifier]];  
  	#pragma clang diagnostic pop  
  	
  	
  	2.不兼容指针类型
  	#pragma clang diagnostic push   
	#pragma clang diagnostic ignored "-Wincompatible-pointer-types"     
	#pragma clang diagnostic pop
	
	
	3.retain cycle
  	#pragma clang diagnostic push  
	#pragma clang diagnostic ignored "-Warc-retain-cycles"  
    self.completionBlock = ^ {  
        ...  
    };  
	#pragma clang diagnostic pop  	
	
	
	4.未使用变量
	#pragma clang diagnostic push   
	#pragma clang diagnostic ignored "-Wunused-variable"   
	int a;   
	#pragma clang diagnostic pop   
	

38.Xcode7 bitcode
	
	bitcode用来干嘛的  自行百度
	build settting -> search "bitcode" ->NO
	目前部分三方库不支持bitcode会报错，如果不用支持apple watch，可以将这个属性给关了噢

39.iOS 9 下打开微信 QQ  支付宝等失败
	
[原因](http://awkwardhare.com/post/121196006730/quick-take-on-ios-9-url-scheme-changes)
	
	解决方法
	
	infoPlist 中 ，新增"LSApplicationQueriesSchemes" -> Array[canOpenURL: failed for URL 的schemes]
	
40.关于可以展开、收起的cell
	
	一定要使cell复用，否则在cell的收起与收缩是，会出现cell的错乱
	
41.SDImage 清理缓存

	//清除key索引的图片  
	- (void)removeImageForKey:(NSString *)key;  
	//清除内存图片  
	- (void)clearMemory;  
	//清除物理缓存  
	- (void)clearDisk;  
	//清除过期物理缓存  
	- (void)cleanDisk;  

42.navigationbar的颜色设置问题

* 导航条的阴影（有一条黑线），    
	[self.navigationController.navigationBar setShadowImage:[UIImage new]];
* tintcolor:
* bartinecolor:
* backgorundImage:

43.uitableviewcell上加载cuicollectionviewcell

	1)- (instancetype)initWithCoder:(NSCoder *)aDecoder 比 - (void)awakeFromNib要先执行，collection的配置只有放在awakeFromNib方法里面执行才能生效；
	2)通过xib来设置collectionview的delegate&datasource是无效的（原因不清楚）
	
44.往cell上加了个collectionview，但是...
	
	collectionviewcell上有一个 加了个image  往image上加了点击事件  报了一个非法调用的错误。后来改用了button，原因不详。。
、	

45.registerClass PK registerNib
	
	tableview & collectionView 都有这两种注册cell的方法  
	然而 今天  我使用registerClass([customcell class]才行 [systemcell class]还会报错，需要指定相应的cell种类才行)  cell不能显示，代理方法也都执行，，好诡异的一件事~~   改用registerClass 才行
	
46.给UICollectionView的HeaderView
	

	
47.改变searchBar placeholder和放大镜的颜色
	
	 UITextField *searchField = [self.searchBar valueForKey:@"_searchField"];
     searchField.textColor = [UIColor whiteColor];
     [searchField setValue:[UIColor whiteColor] forKeyPath:@"_placeholderLabel.textColor"];

	 UIImage *image = [[UIImage imageNamed: @"search-big"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal];
	 UIImageView *iView = [[UIImageView alloc] initWithImage:image];
	 searchField.leftView = iView;

48.关于导航条的返回按钮
	
	
	方式一：
	
	
	
    UIButton *btn = [UIButton buttonWithType:UIButtonTypeCustom];
    btn.frame = CGRectMake(0, 0, 30, 30);
    [btn addTarget:self action:@selector(aimedBack) 	forControlEvents:UIControlEventTouchUpInside];
    [btn setImage:[UIImage imageNamed:@"arrow-left"] 	forState:UIControlStateNormal];
    UIBarButtonItem *leftItem = [[UIBarButtonItem alloc] initWithCustomView:btn];
    self.navigationItem.leftBarButtonItem = leftItem;
	-->这种方式可以自定义返回事件，但是丧失了系统自带的右侧滑动返回功能
	
	方式二：
	[[UIBarButtonItem appearance] setBackButtonTitlePositionAdjustment:UIOffsetMake(NSIntegerMin, NSIntegerMin) 	forBarMetrics:UIBarMetricsDefault];//隐藏返回按钮的标题
    UIImage *backButtonImage = [[UIImage imageNamed:@"arrow-left"] resizableImageWithCapInsets:UIEdgeInsetsMake(0, 30, 0, 0)];
    [[UIBarButtonItem appearance] setBackButtonBackgroundImage:backButtonImage forState:UIControlStateNormal 	barMetrics:UIBarMetricsDefault];
	-->可以通过滑动返回，但是其返回时间不能定制，
	
	
49.解除方法中不使用参数警告

![](/Users/apple/Desktop/QQ20151126-0.png)

	使用__unused 声明一下不用的参数
	
50.桥接（bridge）

	ARC环境下编译器不会自动管理CF对象的内存，所以当我们创建了一个CF对象以后就需要我们使用CFRelease将其手动释放。
	那么CF和OC相互转化的时候该如何管理内存呢？答案就是我们在需要时可以使用__bridge,__bridge_transfer,__bridge_retained
	__bridge:
	系统为我们自动添加了__bridge,因为是OC创建的对象并且在转换时没有涉及对象所有权的转换，所以上面的代码不需要加CFRelease
	__bridge_transfer:
	常用在讲CF对象转换成OC对象时，将CF对象的所有权交给OC对象，此时ARC就能自动管理该内存；
	__bridge_retained:
	常用在将OC对象转换成CF对象时，将OC对象的所有权交给CF对象来管理；(作用同CFBridgingRetain())

51.layer几个主要点坐标：layer.anchor, layer.zpoint,layer.contentsCenter,layer.containtsPoint

52.- (CALayer *)hitTest:(CGPoint)p;

	通过点找到layer
	
53.UIView中的坐标转换

	// 将像素point由point所在视图转换到目标视图view中，返回在目标视图view中的像素值
	- (CGPoint)convertPoint:(CGPoint)point toView:(UIView *)view;
	// 将像素point从view中转换到当前视图中，返回在当前视图中的像素值
	- (CGPoint)convertPoint:(CGPoint)point fromView:(UIView *)view;

	// 将rect由rect所在视图转换到目标视图view中，返回在目标视图view中的rect
	- (CGRect)convertRect:(CGRect)rect toView:(UIView *)view;
	// 将rect从view中转换到当前视图中，返回在当前视图中的rect
	- (CGRect)convertRect:(CGRect)rect fromView:(UIView *)view;

	
	

54.XCode调试技巧

	1）breakPoint + po/print: 可以在指定的位置打上断点，并且在控制台中，通过po/print命令打印出相应的对象；
	2）
	
	
55.preferredMaxLayoutWidth

56.浪费我半天才找出来的小op
	
	SB中，给tableView拖了一个headerView（其实不是真正的headerView），headerView高度改变，下面的tableview相应调整。。。后来发现下面tableView不能动了，查了半天，由于estimatedHeightForRowAtIndexPath方法被注释了。一直以为这个方法没有什么用....
	
57.UITableView reloadSections : header view消失

	学着用用UITableViewHeaderFooterView
	
58.app唤醒，UIWebView调JS，js需要显示UI控件，导致程序挂掉（线程冲突）

59.jsonModel的使用
	
- model需继承JSONModel
- 类型一致。number、string、array、dic要对应;
- 对于不确定是否存在的键值对，使用Optional。例如：@property (nonatomic,retain) NSNumber<Optional> *score;
- 多重model，需要在子model中添加相应类型协议，并且在父类model中，规定model数组的类型
	例如：子model中 @protocol BlockCategoryModel @end 
		父model中 @property (nonatomic,strong) NSArray <BlockCategoryModel>*BlockInfo;
		

60.关于字符串检查

	NSRegularExpression 
	NSPredicate
	NSTextCheckingResult & NSDataDetector
 ===	
	1）正则表达式 -- NSRegularExpression类的使用
	例如: RegularExpression *regex = [NSRegularExpression regularExpressionWithPattern:@"http+:[^\\s]*" options:0 error:&error];
	 
	NSTextCheckingResult *result2 = [regex2 firstMatchInString:textField.text options:0 range:NSMakeRange(0, [textField.text length])];

	2）






