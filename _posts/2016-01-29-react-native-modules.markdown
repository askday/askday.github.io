---
layout: post
title: "react-native自定义原生组件"
date: "2016-01-29 10:32:45"
---
使用react-native的时候能够看到不少函数调用式的组件，像LinkIOS用来呼起url请求

{% highlight html%}
  LinkIOS.openUrl('http://www.163.com');
{% endhighlight %}

actionSheetIOS用来实现ios客户端底部弹起的选择对话框

{% highlight html%}
ActionSheetIOS.showActionSheetWithOptions({
  options: BUTTONS,
  cancelButtonIndex: CANCEL_INDEX,
  destructiveButtonIndex: DESTRUCTIVE_INDEX,
},
(buttonIndex) => {
  this.setState({ clicked: BUTTONS[buttonIndex] });
});
{% endhighlight %}

这些组件的使用方式都大同小异，通过声明一个native module，然后在这个组件内部通过底层实现方法的具体内容
<!-- more -->
像ActionSheetIOS在使用的时候，首先需要在工程的pod库中添加ActionSheetIOS对应的RCTActionSheet

{% highlight html%}
pod 'React', :path => 'node_modules/react-native', :subspecs => [
'Core',
'RCTActionSheet'
# Add any other subspecs you want to use in your project
]
{% endhighlight %}


我们可以看到RCTActionSheet相关的实现的代码是放在react-native/Libraries/ActionSheetIOS下的

![actionSheetIOS](/image/actionSheetIOS.png)

整个工程包含3个代码文件，ActionSheetIOS.js、RCTActionSheetManager.h、RCTActionSheetManager.m

ActionSheetIOS.js内容很简单,先是定义了引用oc代码的方式

{% highlight  javascript%}
  var RCTActionSheetManager = require('NativeModules').ActionSheetManager;
{% endhighlight %}

然后定义了ActionSheetIOS组件，并export
{% highlight javascript %}
var ActionSheetIOS = {
  showActionSheetWithOptions(options: Object, callback: Function) {
    invariant(
      typeof options === 'object' && options !== null,
      'Options must a valid object'
    );
    invariant(
      typeof callback === 'function',
      'Must provide a valid callback'
    );
    RCTActionSheetManager.showActionSheetWithOptions(
      {...options, tintColor: processColor(options.tintColor)},
      callback
    );
  },

  showShareActionSheetWithOptions(
    options: Object,
    failureCallback: Function,
    successCallback: Function
  ) {
    invariant(
      typeof options === 'object' && options !== null,
      'Options must a valid object'
    );
    invariant(
      typeof failureCallback === 'function',
      'Must provide a valid failureCallback'
    );
    invariant(
      typeof successCallback === 'function',
      'Must provide a valid successCallback'
    );
    RCTActionSheetManager.showShareActionSheetWithOptions(
      {...options, tintColor: processColor(options.tintColor)},
      failureCallback,
      successCallback
    );
  }
};
module.exports = ActionSheetIOS;
{% endhighlight %}

我们看到关键是引入底层oc的方式，其他的跟写前端没啥差别

然后再看RCTActionSheetManager的实现
{% highlight object-c%}
#import <UIKit/UIKit.h>

#import "RCTBridge.h"

@interface RCTActionSheetManager : NSObject <RCTBridgeModule>

@end
{% endhighlight %}

主要是实现了RCTBridgeModule这个协议，这个协议是实现前端js-》oc的主要中间件，感兴趣的可以看看实现，然后就是对RCTActionSheetManager的实现的代码，关键几句

{% highlight object-c %}
@implementation RCTActionSheetManager
{
// Use NSMapTable, as UIAlertViews do not implement <NSCopying>
// which is required for NSDictionary keys
NSMapTable *_callbacks;
}

RCT_EXPORT_MODULE()
...
RCT_EXPORT_METHOD(showActionSheetWithOptions:(NSDictionary *)options
                  callback:(RCTResponseSenderBlock)callback)
{
  ...
}
{% endhighlight %}

主要是RCT_EXPORT_MODULE用来注册react-native module ，然后具体的实现方法放在RCT_EXPORT_METHOD开头的函数内

RCT开头的宏用来区分react-native函数与原声的函数，jspatch的bang有过具体分析，感兴趣的可以看看
http://blog.cnbang.net/tech/2698/

所以我们自己实现一个原生的react-native组件的时候，完全可以照着actionSheetIOS来做
在前端自定义一个js，通过require('NativeModules').XXX 引入
然后在底层实现RCTBridgeModule的类，在类里把RCT_EXPORT_MODULE、RCT_EXPORT_METHOD加上即可
