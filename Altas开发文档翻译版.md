## 系统概述

在Atlas中排序

Atlas使用Lua语言进行排序。网上有很多学习Lua的资源。《Lua编程》一书在线免费提供，是学习语言的绝佳资源。

还有关于Lua的Udemy在线课程不是免费的，但可能有用：

Lua编程-掌握基础知识

Lua编程：成为Lua大师

Atlas核心

Atlas Core是一个始终运行的过程，它：

启动Atlas组和检测插件

监控测试状态

向连接的Atlas控制台发送测试状态

与Atlas Launchers通信以启动和停止测试

阿特拉斯集团流程

Atlas组用于对共享资源和设备上的操作进行排序。一个站点上总是必须有一个Atlas Group。



Atlas检测流程

Atlas Detection查找设备和资源（仪器、控制器、固定装置等），并将其发送到相应的Atlas Group。

##  SDK安装

安装Atlas 2.0 SDK

使用Apple B&I构建列车的产品

如果您的产品有设备端软件的B&I培训，可以通过RestoreTools.pkg安装Atlas 2.0。



在开发人员笔记本电脑上，从还原工具安装后，运行sudo/usr/local/Alas/activate。sh激活Atlas。



对于适用于土拨鼠机器的覆盖层，请放置/private/etc/StationSetup/EveryBoot/ToRun/atras2_activate。sh在覆盖层中。通过使用chmod a+x路径/to/file设置权限，确保该文件标记为可执行文件，以便GH SOA在每次启动时都会运行该文件。



无Apple B&I构建列车的产品

通过此雷达获取Atlas 2.0<rdar://problem/48577749>Atlas 2.0版本跟踪器。



在开发人员笔记本电脑上，安装程序负责任何所需的激活。



对于适用于土拨鼠机器的覆盖层，请放置/private/etc/StationSetup/EveryBoot/ToRun/atras2_activate。sh在覆盖层中。通过使用chmod a+x路径/to/file设置权限，确保该文件标记为可执行文件，以便GH SOA在每次启动时都会运行该文件。



开发人员包

Atlas 2.0 Lua调试器和其他特定于开发的工具和功能可在<rdar://problem/48577749>Atlas 2.0版本跟踪器



附加依赖项

运行Atlas 2需要Instant Pudding Library。请仔细阅读安装说明，因为它们没有提供包安装程序，必须将库和配置文件复制到适当的位置。



安装问题

如果由于文件权限导致Atlas2安装失败，则需要允许安装程序创建正确的文件夹。根据您的macOS版本，执行此操作的步骤有所不同。



macOS Sierra（10.12）及更高版本



确保禁用系统完整性保护：



进入恢复模式（机器引导期间的CMD+R）

禁用SIP（打开终端并输入csrutil Disable）

重新启动到macOS

运行Atlas安装程序

macOS Catalina（10.15）及更高版本



Catalina和更新版本的macOS需要手动步骤才能安装Atlas。您需要使用firmlinks设置可编辑/vault文件夹：



下载catalina修复程序。来自释放雷达的sh<rdar://problem/25611403>

运行catalina修复程序。sh将：

确保/系统/卷/数据/保险库存在

将“vault/System/Volumes/Data/vault”附加到/etc/synthental。conf（请注意，vault和/System/Volumes/Data/vault之间有一个选项卡，而不是空格）

确保/System/Volumes/Data/AppleInternal存在

将“AppleInternal/System/Volumes/Data/AppleInternal”附加到/etc/synthental。conf（注意AppleInternal和/System/Volumes/Data/AppleInternal之间有一个制表符，而不是空格）

重新启动计算机

运行Atlas安装程序（可能仍然存在问题，请参阅下面的警告）

## 目录结构

覆盖目录结构

以下是Atlas使用的文件路径及其简短描述。



工作站配置

位置：~/Library/Atlas2/Config/*.plist



站点配置plist文件位于此处。AtlasLauncher start采用此目录中plist的文件名（而不是文件路径）。覆盖所需的其他配置文件也可以存储在这里。



Lua模块

位置：



Atlas在/usr/local/Alas/modules/*.lua中提供的模块

DRI模块位于：

~/Library/Atlas2/Modules/*.lua

~/Library/Atlas2/Modules/*/init.lua

模块是包含助手脚本的Lua文件，这些脚本加载到组/检测序列或动作序列中。模块MyUtils。lua将使用以下语法加载：




需要“MyUtils”

Lua序列

组/检测Lua序列

位置：~/Library/Atlas2/Sequences/*.lua



Atlas Group和Detection序列文件位于此处。两者都是具有主入口点的Lua脚本。Station Configuration plist引用此目录中的Lua脚本文件名（而不是文件路径）。



动作Lua序列

位置：：



~/Library/Atlas2/Actions/*.lua

动作是具有主入口点的Lua脚本，通过指定相对文件名运行。



插件

位置：



Atlas在/usr/local/Alas/plugins/*.l-dylib中提供的插件

~/Library/Atlas2/plugins/*.l-dylib中的DRI插件

插件是可以使用Atlas在Lua中加载的dylib。loadPlugin（“MyPlugin”）语法。



资产

位置：~/图书馆/Atlas2/资产/*



覆盖所需的任何不属于上述类别的资产（例如，plist或csv格式的配置文件或限制文件）都应放在资产文件夹下。要在运行时获取此路径，请使用Atlas.assetsPath。



Lua搜索路径

使用require将首先查找插件，然后查找Lua模块，按照上面定义的每个模块的搜索路径。建议用户在其Plugins/Actions/Modules的基本文件夹下创建子文件夹，以将其工作站资源与其他工作站资源分开。



require调用允许相对于~/Library/Atlas2/Modules的路径，例如：



需要“FACT/utils”

## 使用Apple Silicon在Mac上运行

Atlas2提供胖图像

发布雷达中的Atlas2安装程序附带了包含x86_64和arm64二进制文件的胖图像，因此在带有Apple Silicon的mac上，它可以以两种方式运行：



本机模式：运行二进制文件的arm64片段

Rosetta转换模式：运行x86_64切片。

根据developer.apple上的文档。com，默认情况下，在带有Apple Silicon的Mac上，arm64切片将运行并尝试从其加载的框架/库加载arm64切片。



在创建此文档时（2021 6月），大多数插件和UI捆绑包都是x86_64，没有arm64切片；青蛙还没有构建arm64切片的构建节点。因此，要使用现有的x86_64二进制文件，AtlasCore2需要在x86_4模式下运行。



使用x86_64 InstantPudding/plugins/UI捆绑包运行的步骤

下载launchctl-plist，放入/Library/LaunchAgents，替换同名文件。这迫使AtlasCore2使用Rosetta运行x86_64切片。

重新加载服务

launchctl卸载/Library/LaunchAgents/com.apple.hwte.AtlasCore2.plist

launchctl加载/Library/LaunchAgents/com.apple.hwte.AtlasCore2.plist

设置AtlasUI。应用程序始终以x86_64模式运行，如果使用UI捆绑包，这是必需的

在Finder中，转到/AppleInternal/Applications/并找到AtlasUI.app

右键单击应用程序并选择获取信息；或者选择应用程序并按“cmd+i”。

在弹出的窗口中，选中General中的“Open using Rosetta”框。

关闭AtlasUI。如果应用程序已打开，请重新启动该应用程序。

开始图集测试

AtlasStatusMenu正常工作。

运行没有任何前缀的AtlasLauncher将报告以下错误消息：加载失败

为避免出现此问题，请在启动AtlasLauncher时使用arch-x86_64前缀，如arch-x86 _64 AtlasLauncher start station.plist



#在带有Apple Silicon的Mac上运行AtlasLauncher时的错误消息

加载库/usr/local/lib/libCBAuth时出错。dylib:error=dlopen（/usr/local/lib/libCBAuth.dylib，0x0005）：已尝试：“/usr/local/lib/libCBAuth。dylib”（胖文件，但缺少兼容的体系结构（具有“i386，x86_64”，需要“arm64e”）、“/usr/lib/libCBAuth”。dylib'（无此类文件）

无法加载libInstantPudding.dylib

加载libInstantPudding.dylib:dlopen（/usr/local/lib/libInstantPuding.dylib，0x0004）时出错：已尝试：“/usr/local/lib/libInstancePudding。dylib”（胖文件，但缺少兼容的体系结构（具有“i386，x86_64”，需要“arm64e”），“/usr/lib/libInstantPudding”。dylib'（无此类文件）

在本机模式下运行

通过InstantPudding库支持arm64架构，以及所有基于arm64支持构建的插件/UI包，Atlas2可以在本机模式下运行：



使用Atlas plist启动AtlasCore2而不使用任何“arch”前缀，或使用“arch-arm64”前缀。

取消选择始终运行AtlasUI的选项。Rosetta中的应用程序

使用AtlasLauncher命令行工具，不带前缀或前缀为“arch-arm64”。



Atlas 2.0 Lua插件

Atlas 2.0 Lua插件从位于/usr/local/Alas/plugins/*的dylibs加载到Atlas 2.0Lua VM（发现VM、Atlas Group VM、操作VM）中。l-dylib和~/Library/Atlas2/Plugins/*.l-dylib。每个dylib只包含一个插件，它通过实现以下方法来实现：




id插件上下文构造函数（）；

NSDictionary*插件函数表（）；

NSDictionary*插件常量表（）；

Atlas 2.0 Lua插件也可以加载到远程进程中，以便在Objective-C环境中运行插件代码。远程进程是一个与Atlas系统松散耦合的独立进程，它加载Atlas Lua插件，通过侦听从Lua操作发送的请求、执行插件命令并将结果返回给调用者来释放其服务。如果您的插件是由主机中运行的远程进程加载的，我们分别在主机远程插件和主机远程进程上调用它们，并使用Atlas Group远程托管挂钩来控制它。如果您的插件是由设备中运行的远程进程加载的，我们分别调用设备上的远程插件和设备上的进程，并使用ADCDUTPlugin远程插件来控制它。



Xcode模板

有一个用于创建Atlas 2.0 Lua插件的Xcode模板。



方法

施工单位

PluginContextConstructor（）返回的对象可以是任何Objective-C对象，并由表示插件实例的Lua变量保存。当该变量被垃圾收集时，对象被释放。



功能

PluginFunctionTable（）返回的字典定义了插件在Lua中的方法，以及它们如何调用从PluginContextConstructor（）返回对象的Objective-C方法。有三类方法可以桥接到Lua中：



Lua PluginFunctionTable Objective-C方法

无法返回值@“increment”：@[@[ATKSelector（increment）]]-（void）increment；

错误，没有返回值@“start”：@[@[ATKSelector（start:）]]-（BOOL）start:（NSError**）error；

返回值@“read”错误：@[@[ATKSelector（read:）]]-（id）read:（NSError**）错误；

这些示例都有0个Lua参数，以显示失败和返回值的差异。将参数从Lua传递到方法中的细节将在本页后面描述。



无返回值无效

在这种情况下，该方法不会失败，因此Lua函数不会抛出任何错误，Objective-C函数也没有NSError out参数的返回值。



无返回值的错误

此方法可能失败，因此Objective-C函数有一个布尔返回值和一个NSError out参数作为最后一个参数。如果Objective-C返回YES，那么Lua函数将不会返回任何内容，但会成功运行。如果Objective-C返回NO，则必须填写NSError-out参数，并且将抛出一个Lua错误，其中包含此NSError对象的详细信息。



返回值错误

此方法可能失败，并有返回值。Objective-C函数返回一个id对象，并将NSError out参数作为最后一个参数。如果Objective-C返回一个对象，Lua将获取该对象作为返回值。如果Objective-C返回nil，则表示存在错误，必须填写NSError out参数。然后将引发Lua错误。



如果希望Lua方法调用得到一个零返回值但没有错误，Objective-C函数必须返回[NSNull null]。



Objective-C函数可以被定义为返回比id更具体的内容，以允许编译器查找编码错误。如果函数返回字符串，请将其声明为-（NSString*）getString:（NSError**）error。NS类型到ATK（Lua）类型的映射在参数类型部分。



多个返回值



ATKPack类型可用于向Lua返回多个值。ATKPack PluginFunctionTable条目和函数签名示例：



PluginFunctionTable Objective-C方法

@“multiReturn”：@[@[ATKSelector（multiReturn:）]]-（id）multi返回：（NSError**）错误；

如果这个函数在Objective-C中是这样实现的：




-（id）multiReturn：（NSError**）错误{

返回ATKPack（（@[@（2），@“random”]）；

}

这在Lua中可以这样称呼：




myPlugin=需要“myPlugin”

a、 b=我的插件。multiReturn（）--a=2，b=“随机”

参数

PluginFunctionTable（）字典允许定义每个方法可以使用的参数的数量和类型。



Lua PluginFunctionTable Objective-C方法注释

add（5）@“add”：@[ATKSelector（add：）]，ATKNumber]-（void）add：（NSNumber*）编号；无返回值无效

x=插件。hexToBytes（“12AF”）@“hexToBytes”：@[ATKSelector（hexIntoBytes:错误：），ATKString]-（NSData*）hexInto字节：（NSString*）十六进制错误：（NSError**）错误；NSData返回值出错

可以重载Lua方法以处理多个不同的参数。以下PluginFunctionTable条目：



@“写入”：@[

@[ATKSelector（writeCharacters：错误：），ATKArray]，

@[ATKSelector（writeString:错误：），ATKString]

]

将映射到以下Objective-C函数：




-（id）writeCharacter:（NSArray*）字符错误：（NSError**）错误；

-（id）writeString:（NSString*）字符串错误：（NSError**）错误；

允许Lua调用：




写作（“mystring”）

写（{“m”，“y”，“c”，“h”，“a”，“r”，“s”}）

方法名称限制



方法名称只能包含字母数字和/或下划线字符，第一个字符不能是数字。



参数类型

创建ATKSelector对象时，ATK类型映射到NS类型，如下表所示。



ATK类型目标C类型注释

ATK编号NSNumber

ATK字符串NSString

ATKArray NSArray阵列

ATK词典

ATKAny NSObject公司

ATKType请参阅注释ATKType（NSData）只接受NSData类型

ATKVariadic参见注释方法的变量参数

ATKProtocol参见注释ATKProto（＜ProtocolName＞）参见下面的示例

ATK协议示例



通过接受ATKLuaPluginProtocol协议，此类型可用于将Atlas 2.0 Lua插件接受为其他插件。



以下PluginFunctionTable条目：




@“创建”：@[@[ATKSelector（createSuperWidget:error:），ATKProtocol（ATKLuaPluginProtocol）]]

将映射到以下Objective-C函数：




-（id）createSuperWidget:（ATKLuaPlugin*）小部件错误：（NSError**）错误；

允许Lua调用：




widget=createWidget（）--返回Atlas 2.0插件

superWidget=createSuperWidget（小部件）——将Atlas2.0插件作为参数，并返回一个新插件

ATKProtocol对象的生存期



对小部件的引用仅在createSuperWidget（）调用期间有效。此引用不能在内部保存并在以后使用。



常量

PluginConstantTable（）返回的字典将映射到加载插件时返回的Lua对象上。



此示例PluginConstantTable（）将生成常量PluginName。加载插件时，Lua中的PI可用。




NSDictionary*插件常量表（）

{

返回@{

@“PI”：@M_PI

};

}

目标C↔ Lua数据类型转换

ObjC类型Lua类型注释

NSString字符串

带bool bool的NSNumber编号

NSNumber numberWithInt整数

NSNumber number With float浮点

NSNull无

NSDictionary Lua表具有字符串索引的Lua表转换为NSDictional

NSArray Lua表空Lua表和具有整数索引的Lua表转换为NSArray

ATIPackedReturnType多个返回值

所有其他ObjC类不透明指针

例子：




本地myLuaTable={

property1=“value1”，

property2=“value2”，

属性3={

subProperty1=“值3”

}

}



myPlugin.myPluginMethod（myLuaTable）

当myLuaTable作为参数传递给ObjC插件时，property1和property2都可以用作字符串键来查询值：




-（BOOL）myPluginMethod:（NSDictionary*）myLuaTable

{

NSString*val1=myLuaTable[@“property1”]；

NSString*val2=myLuaTable[@“property2”]；



//要在一个函数调用中访问value3，我们可以使用：

NSString*val3=[myLuaTable valueForKeyPath:@“property3.subProperty1”]；

}

## Xcode Plugin Template

The Xcode template for the creation of Atlas 2.0 plugins is installed with the [Developer Package](../getting-started/sdk-installation.html#developer-package). It will show up in the "Project Templates" section of Xcode when you create a new project.

![image](xcode_newproject.png)

![Xcode template](xcode_template.png)

The `Atlas 2.0 Lua Plugin` Xcode template will give you a base starting point for creating your plugin with pre-populated `PluginContextConstructor()`, `PluginFunctionTable()`, and `PluginConstantTable()` methods.

**Methods Populated From Xcode Template**





```
#pragma mark -
#pragma mark Plugin Entry Point Functions

id PluginContextConstructor()
{
    return [TestPlugin new];
}

NSDictionary *PluginFunctionTable()
{
    NSDictionary *fTable = @{
        // @"functionName" : @[ @[ ATKSelector(functionSignature), <arguments> ] ]
    };

    return fTable;
}

NSDictionary *PluginConstantTable()
{
    return @{
        //@"PI" : @M_PI
    };
}
```

[PreviousIntroduction](atlas-plugins/atlas-plugins.html)[
  ](atlas-ui/atlas-ui.html)

# Atlas UI

@“写入”：@[

@[ATKSelector（writeCharacters：错误：），ATKArray]，

@[ATKSelector（writeString:错误：），ATKString]

]

将映射到以下Objective-C函数：




-（id）writeCharacter:（NSArray*）字符错误：（NSError**）错误；

-（id）writeString:（NSString*）字符串错误：（NSError**）错误；

允许Lua调用：




写作（“mystring”）

写（{“m”，“y”，“c”，“h”，“a”，“r”，“s”}）

方法名称限制



方法名称只能包含字母数字和/或下划线字符，第一个字符不能是数字。



参数类型

创建ATKSelector对象时，ATK类型映射到NS类型，如下表所示。



ATK类型目标C类型注释

ATK编号NSNumber

ATK字符串NSString

ATKArray NSArray阵列

ATK词典

ATKAny NSObject公司

ATKType请参阅注释ATKType（NSData）只接受NSData类型

ATKVariadic参见注释方法的变量参数

ATKProtocol参见注释ATKProto（＜ProtocolName＞）参见下面的示例

ATK协议示例



通过接受ATKLuaPluginProtocol协议，此类型可用于将Atlas 2.0 Lua插件接受为其他插件。



以下PluginFunctionTable条目：




@“创建”：@[@[ATKSelector（createSuperWidget:error:），ATKProtocol（ATKLuaPluginProtocol）]]

将映射到以下Objective-C函数：




-（id）createSuperWidget:（ATKLuaPlugin*）小部件错误：（NSError**）错误；

允许Lua调用：




widget=createWidget（）--返回Atlas 2.0插件

superWidget=createSuperWidget（小部件）——将Atlas2.0插件作为参数，并返回一个新插件

ATKProtocol对象的生存期



对小部件的引用仅在createSuperWidget（）调用期间有效。此引用不能在内部保存并在以后使用。



常量

PluginConstantTable（）返回的字典将映射到加载插件时返回的Lua对象上。



此示例PluginConstantTable（）将生成常量PluginName。加载插件时，Lua中的PI可用。




NSDictionary*插件常量表（）

{

返回@{

@“PI”：@M_PI

};

}

目标C↔ Lua数据类型转换

ObjC类型Lua类型注释

NSString字符串

带bool bool的NSNumber编号

NSNumber numberWithInt整数

NSNumber number With float浮点

NSNull无

NSDictionary Lua表具有字符串索引的Lua表转换为NSDictional

NSArray Lua表空Lua表和具有整数索引的Lua表转换为NSArray

ATIPackedReturnType多个返回值

所有其他ObjC类不透明指针

例子：




本地myLuaTable={

property1=“value1”，

property2=“value2”，

属性3={

subProperty1=“值3”

}

}



myPlugin.myPluginMethod（myLuaTable）

当myLuaTable作为参数传递给ObjC插件时，property1和property2都可以用作字符串键来查询值：




-（BOOL）myPluginMethod:（NSDictionary*）myLuaTable

{

NSString*val1=myLuaTable[@“property1”]；

NSString*val2=myLuaTable[@“property2”]；



//要在一个函数调用中访问value3，我们可以使用：

NSString*val3=[myLuaTable valueForKeyPath:@“property3.subProperty1”]；

}

## Atlas 2.0 UI StationInfo View Plugin

这个插件纯粹是一个数据接收器，您只需要显示有关站点的信息。这可用于显示恢复站的捆绑信息和过期时间，以满足审核要求及其在传感器站上的状态。



该视图的固定高度为56个单位，宽度可配置为最小允许值310个单位。



插件要求

这里描述了所有AtlasUI视图插件的常见要求。



对于StationInfo，您的主要班级需要遵循以下协议：




@协议ATKExtensionStationInfoView＜NSObject＞



//作为NSViewController的子类已经满足了这些要求

@属性（强）NSView*视图；

-（实例类型）initWithNibName:（可为Null的NSNibName）nibNameOrNil束：（可为null的NSBundle*）nibBundleOrNil；



//这是您从集团流程接收数据的方式

-（void）updateGroupIndex:（NSUInteger）groupIndex groupInfo:（NSDictionary*）groupInfo；



//允许您控制为视图提供最小宽度约束（高度固定为60）

@可选

-（CGFloat）最小宽度提示；



@结束

Xcode模板

用于创建AtlasUI状态视图插件的Xcode模板与开发包一起安装。当您创建新项目时，它将显示在Xcode的“Atlas 2.0”部分。



Xcode模板

Atlas 2.0 UI StationInfo插件Xcode模板将为您提供一个基本的起点，用于使用预填充的主体类源文件、xib文件和一些基本方法创建StationInfo插件。

## Atlas 2.0 UI Status View Plugin

This plugin is purely a data sink where you are just displaying the status of all the running units. AtlasUI ships a default StatusTable.bundle that uses a tableview for visualization. To reduce duplication of data management AtlasUIExtension contains classes called ATKStatusModel and ATKUnitStatus which are KVC and KVO compliant making use of bindings super easy in the view plugin. The end result is simplification in creating a custom view plugin where you are creating the views and binding the view controllers to appropriate properties. With this approach everything will work magically without needing to write code that to manage any updates to the properties manually.

AtlasUI ships with the following Status View plugins that can be selected using the `StatusViewPlugin` key-value pair under `AtlasUI` config. The default Status View plugin is `StatusTable.bundle`.

- [StatusTable.bundle](status-view-status-table.html)
- [StatusCollection.bundle](status-view-status-collection.html)

## Plugin Requirements

Common requirements for all AtlasUI view plugins are described [here](atlas-ui.html#plugin-requirements).

For Status plugin specifically your principal class will need to follow below protocol :



```
@protocol ATKExtensionStatusView <NSObject>

// Being subclass on NSViewController already fulfills these requirements
@property (strong) NSView *view;
- (instancetype)initWithNibName:(nullable NSNibName)nibNameOrNil bundle:(nullable NSBundle *)nibBundleOrNil;

// Primarily used by Interactive Plugin to highlight corresponding systemIndex
// These are not guaranteed to be called on mainThread so make sure you dispatch accordingly for UI work
- (void)startHighlightingSystemIndex:(NSUInteger)systemIndex;
- (void)stopHighlightingSystemIndex:(NSUInteger)systemIndex;

// Gives you control to provide minimum height/width constraints for your view
@optional
- (CGFloat)minHeightHint;
- (CGFloat)minWidthHint;

@end
```

## Xcode Template

The Xcode template for the creation of AtlasUI Status View plugins is installed with the [Developer Package](../getting-started/sdk-installation.html#developer-package). It will show up in the "Atlas 2.0" section of Xcode when you create a new project.

![Xcode template](../atlas-plugins/xcode_template.png)

The `Atlas 2.0 UI Status Plugin` Xcode template will give you a base starting point for creating your Status plugin with pre-populated principal class source files, xib file and some basic methods.

## StatusTable Status View Plugin

This is a StatusView plugin that ships as part of AtlasUI. This shows status of the units in a table form.

StatusTable supports following keys in the `StatusViewConfig` dictionary.

| Key                     | Type    | Notes                                   |
| ----------------------- | ------- | --------------------------------------- |
| **`HideGroupNames`**    | boolean | hides `GROUP` column in status table    |
| **`HideSlotNames`**     | boolean | hides `SLOT` column in status table     |
| **`HideBaseDirectory`** | boolean | hides `DIR` column in status table      |
| **`HideLogFilePath`**   | boolean | hides `LOG` column in status table      |
| **`HideDuration`**      | boolean | hides `DURATION` column in status table |

![StatusTable](atlas-ui-status-table-input-scan.png)

## StatusCollection Status View Plugin

这个插件纯粹是一个数据接收器，您只需要显示有关站点的信息。这可用于显示恢复站的捆绑信息和过期时间，以满足审核要求及其在传感器站上的状态。



该视图的固定高度为56个单位，宽度可配置为最小允许值310个单位。



插件要求

这里描述了所有AtlasUI视图插件的常见要求。



对于StationInfo，您的主要班级需要遵循以下协议：




@协议ATKExtensionStationInfoView＜NSObject＞



//作为NSViewController的子类已经满足了这些要求

@属性（强）NSView*视图；

-（实例类型）initWithNibName:（可为Null的NSNibName）nibNameOrNil束：（可为null的NSBundle*）nibBundleOrNil；



//这是您从集团流程接收数据的方式

-（void）updateGroupIndex:（NSUInteger）groupIndex groupInfo:（NSDictionary*）groupInfo；



//允许您控制为视图提供最小宽度约束（高度固定为60）

@可选

-（CGFloat）最小宽度提示；



@结束

Xcode模板

用于创建AtlasUI状态视图插件的Xcode模板与开发包一起安装。当您创建新项目时，它将显示在Xcode的“Atlas 2.0”部分。



Xcode模板



Atlas 2.0 UI StationInfo插件Xcode模板将为您提供一个基本的起点，用于使用预填充的主体类源文件、xib文件和一些基本方法创建StationInfo插件。

## InputScan Interactive View Plugin

