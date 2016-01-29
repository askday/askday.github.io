---
layout: post
title: "使用eventEmitter在react-native中实现导航条事件"
date: "2016-01-28 19:42:07"
---

react-native中导航条组件可以使用navigatorIOS vs navigator,前者不是facebook公开维护的，但是使用起来比较方便

所以navigatorIOS目前在ios平台用的很普遍，这里主要是来实现navigatorIOS上添加rightButton后，在navigator的组件里添加点击事件

navigatorIOS的声明方式：

{%highlight  javascript%}
<NavigatorIOS
  style={styles.wrapper}
  tintColor="white"
  titleTextColor="white"
  translucent={true}
  initialRoute=\{\{
    component: UserView,
    title: '我的',
    navigationBarHidden:true,
    passProps: {myProp: 'foo'},
\}\}>
{%endhighlight%}

那么UserView就是这个页面的根页面，而且此时UserView的导航条是隐藏的
<!-- more -->
此时从UserView进入其他的页面的时候，同样要使用navigator，使用方法如下：

{%highlight  javascript%}
this.props.navigator.push({
  title: itemTitles[sectionID][rowID],
  component: toView,
  rightButtonTitle:'说明',
  onRightButtonPress: this.onRightButtonPress,
  navigationBarHidden:false,
  passProps:{events: this.eventEmitter}
});
{%endhighlight%}

此时在toView页面可以看到导航条，且导航条的rightButton上面含有说明两个字

rightButton有一个点击事件，实在UserView中实现的

{%highlight  javascript%}
onRightButtonPress: function() {
  console.log('onRightButtonPress');
  this.eventEmitter.emit('onRightButtonPress', null);
}
{%endhighlight%}

这时候可以看到我们使用了eventEmitter变量，这个变量是在什么时候声明的呢？

是在UserView初始化的时候，如下

{%highlight  javascript%}
componentWillMount: function() {
  this.eventEmitter = new EventEmitter();
},
{%endhighlight%}

到这在UserView中要做的准备工作就完成了，下一步需要到toView中注册onRightButtonPress

{%highlight  javascript%}
componentWillMount:function(){
  this.addListenerOn(this.props.events,'onRightButtonPress', this.rightButtonClick);
},
{%endhighlight%}

然后在toView中实现rightButtonClick方法
{%highlight  javascript%}
rightButtonClick:function(){
  console.log('right click');
},
{%endhighlight%}

这时候就在react-native中通过注入事件实现了导航条的点击事件。
