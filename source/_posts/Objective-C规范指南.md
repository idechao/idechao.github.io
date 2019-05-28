---
title: Objective-C规范指南
date: 2019-05-27 22:18:32
tags: iOS
---


## 点语法
应该 **始终** 使用点语法来访问或者修改属性，访问其他实例时首选括号（对比下面的属性）。

推荐：

```
view.backgroundColor = [UIColor orangeColor];
 
[UIApplication sharedApplication].delegate;
```

反对：

```
[view setBackgroundColor:[UIColor orangeColor]];

UIApplication.sharedApplication.delegate;
```
	
## 间距
* 一个缩进使用 4 个空格，永远不要使用制表符（tab）缩进。请确保在 Xcode 中设置了此偏好。
* 方法的大括号和其他的大括号（if/else/switch/while 等等）始终和声明在同一行开始，在新的一行结束。

推荐：

```
if (user.isHappy) {
// Do something
}
else if (user.isOld) {
// Do something
}		
else {
// Do something else
}
```
	
* 方法之间应该正好空一行，这有助于视觉清晰度和代码组织性。在方法中的功能块之间应该使用空白分开，但往往可能应该创建一个新的方法。
* @synthesize 和 @dynamic 在实现中每个都应该占一个新行。

## 条件判断
条件判断主体部分应该始终使用大括号括住来防止出错，即使它可以不用大括号（例如它只需要一行）。这些错误包括添加第二行（代码）并希望它是 if 语句的一部分时。还有另外一种[更危险的](http://programmers.stackexchange.com/questions/16528/single-statement-if-block-braces-or-no/16530#16530)，当 if 语句里面的一行被注释掉，下一行就会在不经意间成为了这个 if 语句的一部分。此外，这种风格也更符合所有其他的条件判断，因此也更容易检查。

推荐：

```
if (!error) {
	return success;
}
```
	
反对：

```
if (!error)
	return success;
```
或：

```
if (!error) return success;
```
	
##三目运算符
三目运算符(? :) ，只有当它可以增加代码清晰度或整洁时才使用。单一的条件都应该优先考虑使用。多条件时通常使用 if 语句会更易懂，或者重构为实例变量。

推荐：

```
result = a > b ? x : y;
```
	
反对：

```
result = a > b ? x = c > d ? c : d : y;
```
	
##错误处理
当引用一个返回错误参数（error parameter）的方法时，应该针对返回值，而非错误变量。

推荐：

```
NSError *error;
if (![self trySomethingWithError:&error]) {
// 处理错误
}
```
	
反对：

```
NSError *error;
[self trySomethingWithError:&error];
if (error) {
// 处理错误
}
```
	
一些苹果的 API 在成功的情况下会写一些垃圾值给错误参数（如果非空），所以针对错误变量可能会造成虚假结果（以及接下来的崩溃）。

##方法
在方法签名中，在 -/+ 符号后应该有一个空格。方法片段之间也应该有一个空格。构造方法使  *[instancetype ](http://clang.llvm.org/docs/LanguageExtensions.html#related-result-types)* 作为返回类型来代替 *id* 。

推荐：

```
- (void)setExampleText:(NSString *)text image:(UIImage *)image;
```

对于私有方法，应该加前缀用以区分。具体使用可以自行决定，建议使用p加下划线的方式：*p_* , p表示"private"，不建议使用单个下划线的方式，这种方式是预留给苹果使用的。
	
##变量
变量名应该尽可能命名为描述性的。除了 for() 循环外，其他情况都应该避免使用单字母的变量名。 星号表示指针属性变量，例如：`NSString *text`不要写成` NSString* text `或者`NSString * text `，常量除外。 

尽量定义属性来代替直接使用实例变量,同时声明内存的管理方式。如果一个属性只在 *init* 方法里设置了一次，声明为 *readonly* 。

推荐：

```
@interface WRGSection: NSObject

@property (nonatomic, strong) NSString *headline;

@end
```
	
反对：

```
@interface WRGSection : NSObject {
	NSString *headline;
}
```
	
或：

```
@interface WRGSection: NSObject

@property (nonatomic) NSString *headline;

@end
```
	
###变量限定符
当涉及到[在 ARC 中被引入](https://developer.apple.com/library/ios/releasenotes/ObjectiveC/RN-TransitioningToARC/Introduction/Introduction.html#//apple_ref/doc/uid/TP40011226-CH1-SW4)变量限定符时， 限定符 (__strong, __weak, __unsafe_unretained, __autoreleasing) 应该位于最前面，如下。

```
__weak NSString *text
```


##命名
尽可能遵守苹果的命名约定，尤其那些涉及到[内存管理规则](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)，（[NARC](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/MemoryMgmt.html)）的。

长的和描述性的方法名和变量名都不错。

推荐：

```
UIButton *settingsButton;
```
	
反对：

```
UIButton *setBut;
```
	
类名和常量应该始终使用三个字母的前缀（例如 WRG）（常亮也可使用字母k开头），但 Core Data 实体名称可以省略。为了代码清晰，常量应该使用相关类的名字作为前缀并使用驼峰命名法。

推荐：

```
static const NSTimeInterval WRGArticleViewControllerNavigationFadeAnimationDuration = 0.3;
```
	
反对：

```
static const NSTimeInterval fadetime = 1.7;
```
	
属性和局部变量应该使用驼峰命名法并且首字母小写。

为了保持一致，实例变量应该使用驼峰命名法命名，并且首字母小写，以下划线为前缀。这与 LLVM 自动合成的实例变量相一致。 如果 **LLVM** 可以自动合成变量，那就让它自动合成。

推荐：

```
@synthesize descriptiveVariableName = _descriptiveVariableName;
```
	
反对：

```
id varnm;
```
	
##注释
当需要的时候，注释应该被用来解释 为什么 特定代码做了某些事情。所使用的任何注释必须保持最新否则就删除掉。

通常应该避免一大块注释，代码就应该尽量作为自身的文档，只需要隔几行写几句说明。这并不适用于那些用来生成文档的注释。

##init 和 dealloc
*dealloc* 方法应该放在实现文件的最上面，并且刚好在 *@synthesize* 和 *@dynamic* 语句的后面。在任何类中，*init* 都应该直接放在 *dealloc* 方法的下面。

init 方法的结构应该像这样：

```
- (instancetype)init {
	self = [super init]; // 或者调用指定的初始化方法
	if (self) {
    	// Custom initialization
	}

	return self;
}
```
	
##字面量
每当创建 *NSString*， *NSDictionary*， *NSArray*，和 *NSNumber* 类的不可变实例时，都应该使用字面量。要注意 *nil* 值不能传给 *NSArray* 和 *NSDictionary* 字面量，这样做会导致崩溃。

推荐：

```
NSArray *names = @[@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul"];
NSDictionary *productManagers = @{@"iPhone" : @"Kate", @"iPad" : @"Kamal", @"Mobile Web" : @"Bill"};
NSNumber *shouldUseLiterals = @YES;
NSNumber *buildingZIPCode = @10018;
```
	
反对：

```
NSArray *names = [NSArray arrayWithObjects:@"Brian", @"Matt", @"Chris", @"Alex", @"Steve", @"Paul", nil];
NSDictionary *productManagers = [NSDictionary dictionaryWithObjectsAndKeys: @"Kate", @"iPhone", @"Kamal", @"iPad", @"Bill", @"Mobile Web", nil];
NSNumber *shouldUseLiterals = [NSNumber numberWithBool:YES];
NSNumber *buildingZIPCode = [NSNumber numberWithInteger:10018];
```
	
##CGRect 函数
当访问一个 CGRect 的 x， y， width， height 时，应该使用[CGGeometry 函数](https://developer.apple.com/library/ios/documentation/GraphicsImaging/Reference/CGGeometry/index.html)代替直接访问结构体成员。苹果的 CGGeometry 参考中说到：

```
All functions described in this reference that take CGRect data structures as inputs implicitly standardize those rectangles before calculating their results. 
For this reason, your applications should avoid directly reading and writing the data stored in the CGRect data structure. 
Instead, use the functions described here to manipulate rectangles and to retrieve their characteristics.
```

推荐：

```
CGRect frame = self.view.frame;

CGFloat x = CGRectGetMinX(frame);
CGFloat y = CGRectGetMinY(frame);
CGFloat width = CGRectGetWidth(frame);
CGFloat height = CGRectGetHeight(frame);
```
	
反对：

```
CGRect frame = self.view.frame;

CGFloat x = frame.origin.x;
CGFloat y = frame.origin.y;
CGFloat width = frame.size.width;
CGFloat height = frame.size.height;
```
	
## 常量
常量首选内联字符串字面量或数字，因为常量可以轻易重用并且可以快速改变而不需要查找和替换。常量应该声明为 static 常量而不是 #define ，除非非常明确地要当做宏来使用。

推荐：

```
static NSString * const WRGAboutViewControllerAuthorName = @"Warning";

static const CGFloat WRGImageThumbnailHeight = 50.0;
```
	
反对：

```
#define AuthorName @"Warning"
#define thumbnailHeight 2
```
	
若常量局限于某个实现文件，则以k开头；若在其他类中可见，则以类名为前缀。
	
## 枚举类型
当使用 enum 时，建议使用新的基础类型规范，因为它具有更强的类型检查和代码补全功能。现在 SDK 包含了一个宏来鼓励使用使用新的基础类型 - NS_ENUM()

推荐：

```
typedef NS_ENUM(NSInteger, WRGAdRequestState) {
	WRGAdRequestStateInactive,
	WRGAdRequestStateLoading
};
```
	
凡是需要按位或操作来组合的枚举都应该使用NS_OPTIONS定义。

在switch语句中，总是习惯加上default语句，然而，若是用枚举来定义状态机，则最好不要有default分支。这样增加了一种状态，编译器会发出警告提示需要增加新的处理。
	
## 位掩码
当用到位掩码时，使用 NS_OPTIONS 宏。

举例：

```
typedef NS_OPTIONS(NSUInteger, WRGAdCategory) {
	WRGAdCategoryAutos      = 1 << 0,
	WRGAdCategoryJobs       = 1 << 1,
	WRGAdCategoryRealState  = 1 << 2,
	WRGAdCategoryTechnology = 1 << 3
};
```
	
## 属性
在对象外部访问实例变量的时候，总是应该通过属性来访问。除了几种特殊情况，在对象内部时读取数据时，应该直接访问实例变量，而写入数据时，则应该通过属性来写。这么写的目的是：

* 直接访问实例变量的速度比较快，编译器所生成的代码会直接访问保存对象实例变量的内存。
* 直接访问实例变量时，不会调用其"设置方法"，这就绕过了为相关属性所定义的"内存管理语意"。
* 直接访问实例变量，不会触发KVO通知。具体有没有影响需要看具体行为。
* 通过属性来访问有助于排查错误，可以增加断点来监控对象行为。

在初始化方法和dealloc方法以及getter和setter中，应该总是使用实例变量来读写数据。

使用惰性初始化方法时，使用属性来访问。

私有属性应该声明在类实现文件的延展（匿名的类目）中。

支持：

```
@interface Counter : NSObject

@property (nonatomic, retain) NSNumber *count;

@end

- (NSNumber *)count {
	return _count;
}
	
- (void)setCount:(NSNumber *)newCount {
    [newCount retain];
    [_count release];
    // Make the new assignment.
    _count = newCount;
}

- init {
    self = [super init];
    if (self) {
        _count = [[NSNumber alloc] initWithInteger:0];
    }
    return self;
}
	
- initWithCount:(NSNumber *)startingCount {
    self = [super init];
    if (self) {
        _count = [startingCount copy];
    }
    return self;
}

- (void)dealloc {
    [_count release];
    [super dealloc];
}
```
	
在继承父类的时候，如果本类中没有相关属性，在 init 方法中使用点语法，则会寻找父类属性，而使用实例变量则不会，可以很好的控制本类中的属性，检查属性。

有关在初始化方法和 dealloc 方法中使用访问器方法的信息，参见[这里](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/MemoryMgmt/Articles/mmPractical.html#//apple_ref/doc/uid/TP40004447-SW6)。

## 布尔
因为 nil 解析为 NO，所以没有必要在条件中与它进行比较。永远不要直接和 YES 进行比较，因为 YES 被定义为 1，而 BOOL 可以多达 8 位。

这使得整个文件有更多的一致性和更大的视觉清晰度。

推荐：

```
if (!someObject) {
}
```
	
反对：

```
if (someObject == nil) {
}
```
	
对于 BOOL 来说, 这有两种用法:

```
if (isAwesome)
if (![someObject boolValue])
```
	
反对：

```
if ([someObject boolValue] == NO)
if (isAwesome == YES) // 永远别这么做
```
	
如果一个 BOOL 属性名称是一个形容词，属性可以省略 “is” 前缀，但为 get 访问器指定一个惯用的名字，例如：

```
@property (assign, getter=isEditable) BOOL editable;
```
	
内容和例子来自 [Cocoa 命名指南](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/CodingGuidelines/Articles/NamingIvarsAndTypes.html#//apple_ref/doc/uid/20001284-BAJGIIJE) 。

## 单例
单例对象应该使用线程安全的模式创建共享的实例。

```
+ (instancetype)sharedInstance {
	static id sharedInstance = nil;
	static dispatch_once_t onceToken;
	dispatch_once(&onceToken, ^{
		sharedInstance = [[self alloc] init];
	});
	
	return sharedInstance;
}
```
	
这将会预防[有时可能产生的许多崩溃](http://cocoasamurai.blogspot.com/2011/04/singletons-your-doing-them-wrong.html)。

## 导入
将 *import* 和其他的文件名之间加一个空格。如果有一个以上的 import 语句，就对这些语句进行[分组](http://ashfurrow.com/blog/structuring-modern-objective-c/)。每个分组的注释是可选的。
注：对于模块使用 [@import](http://clang.llvm.org/docs/Modules.html#using-modules) 语法。

除了子类化或是协议之外，最好使用 **@class** 这种方式，避免过多的头文件引入。在引入协议的时候，如果不是连当前类也引入的情况下，将协议单独声明出来再引入。

## Xcode 工程
为了避免文件杂乱，物理文件应该保持和 Xcode 项目文件同步。Xcode 创建的任何组（group）都必须在文件系统有相应的映射。为了更清晰，代码不仅应该按照类型进行分组，也可以根据功能进行分组。

如果可以的话，尽可能一直打开 target Build Settings 中 "Treat Warnings as Errors" 以及一些[额外的警告](http://boredzo.org/blog/archives/2009-11-07/warnings)。如果你需要忽略指定的警告,使用 [Clang 的编译特性](http://clang.llvm.org/docs/UsersManual.html#controlling-diagnostics-via-pragmas) 。

## 空行
1、在 *extension* 和 *implementation* 之间添加一行空格。

推荐：

```
@interface MyClass ()

// Properties - empty line above and below

@end

@implementation MyClass

// Body - empty line above and below

@end
```
	
2、在 *@end*之后添加一空行

3、使用 *pragma mark*之后添加一行空格

```
- (CGSize)intrinsicContentSize {
	return CGSizeMake(12, 12);
}

#pragma mark - Private

- (void)setup {
    
}
```
	
4、操作数学运算符时在运算符俩侧添加空格。一元运算符不用。

推荐：

```
NSInteger index = rand() % 50 + 25;
index++;
index += 1;
index--;
```

5、在进行逻辑判断时，在 *if* 之后添加一个空格，并在 *{* 之前添加一个空格。

推荐：

```
if (alpha + beta <= 0) && (kappa + phi > 0) {
}
```
	
6、对于多参数的方法，除非方法签名大于或等于3行，否则不要换行。

推荐：

```
	// blocks are easily readable
	[UIView animateWithDuration:1.0 animations:^{
    	// something
	} completion:^(BOOL finished) {
    	// something
	}];
```

反对：

```
// colon-aligning makes the block indentation wacky and hard to read
[UIView animateWithDuration:1.0
		 animations:^{
                 	// something
             	}
             	completion:^(BOOL finished) {
                 	// something
             	}];
```
                 	
7、不要在对象类型前和 *protocol*之间添加空格。

推荐：

	@property (nonatomic, weak) id<SGOAnalyticsDelegate> analyticsDelegate;
	
反对：

	@property (nonatomic, weak) id <SGOAnalyticsDelegate> analyticsDelegate;	

## 其他Objective-C 风格指南
如果感觉不太符合口味，可以看看下面的风格指南：

* [Objective-C 编程语言](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/ProgrammingWithObjectiveC/Introduction/Introduction.html)
* [Cocoa 基本原理指南](https://developer.apple.com/legacy/library/documentation/Cocoa/Conceptual/CocoaFundamentals/Introduction/Introduction.html)
* [Cocoa 编码指南](https://developer.apple.com/library/mac/#documentation/Cocoa/Conceptual/CodingGuidelines/CodingGuidelines.html)
* [iOS 应用编程指南](https://developer.apple.com/library/ios/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Introduction/Introduction.html)
* [纽约时报 移动团队 Objective-C 规范指南](https://github.com/NYTimes/objective-c-style-guide)
* [Robots & Pencils Objective-C Style Guide](https://github.com/RobotsAndPencils/objective-c-style-guide)

其他

* [raywenderlich.com](https://github.com/raywenderlich/objective-c-style-guide)
* [Google](http://google-styleguide.googlecode.com/svn/trunk/objcguide.xml)
* [GitHub](https://github.com/github/objective-c-style-guide)
* [Adium](https://trac.adium.im/wiki/CodingStyle)
* [Sam Soffes](https://gist.github.com/soffes/812796)
* [CocoaDevCentral](http://cocoadevcentral.com/articles/000082.php))
* [Marcus Zarra](http://www.cimgf.com/zds-code-style-guide/)

