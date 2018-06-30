---
title: React Native 封装Android UI组件
date: 2017-01-17 11:53:07
tags: React Native
---

移动APP上用的最多的功能之一可能就是下拉刷新了，今天就用` React Native`来封装一个基于[android-Ultra-Pull-To-Refresh](https://github.com/liaohuqiu/android-Ultra-Pull-To-Refresh)的下拉刷新控件。

### 封装流程

1. 继承`SimpleViewManager`或`ViewGroupManager`创建视图管理器类；
2. 继承`ReactPackage`创建自定义ReactPackage，重写`createNativeModules`、`createJSModules`、`createViewManagers`这三个方法，返回类型都是 List 类型， 在`createViewManagers`中添加我们创建好的视图管理器类到集合中，另外两个方法可以返回空 List 集合；
3. 在Application 中添加自定义的 ReactPackage；
4. JS 层封装自定义 UI 控件。

首先继承`ViewGroupManager<T>`创建一个`ReactPtrAndroidManager`，`ViewGroupManager`所需的泛型类型是一个 `ViewGroup`，在这里就是我们的`PtrClassicFrameLayout`（我这里直接使用这个库提供的经典的下拉刷新样式），然后重写以下方法：

* `getName`：返回这个视图管理器的名称，这个名称将被用于在 JavaScript 中创建 RN 组件时所引用。简单地说就是 JS 层创建的刷新组件将通过这个名字来和 Java 层起到映射关系。
* `createViewInstance`：返回一个和泛型类型一致的 `View` 实例。在这里就是返回`PtrClassicFrameLayout`类型的实例。
* `getCommandsMap`：返回想要接收的指令的集合。简单地说就是如果你希望 Java 层接受某个指令，就在这个方法里添加进去，比方说我们这个例子，我想在 Java 层处理自动刷新、刷新完成，那就把自动刷新的Key,Value和刷新完成的 Key,Value添加到 Map 集合中。然后在 JS 层就可以通过调用`UIManagerModule`类中的`dispatchViewManagerCommand`方法，传入对应的指令ID 来调用 Java 层的事件。在 RN 中就是这样实现在 JS 层调用 Java 层的代码。
* `receiveCommand`：这个和`getCommandsMap`是对应的，处理接收到对应指令后的逻辑。我们在`getCommandsMap`中已经添加了一些指令的集合，并且在 JS 层可以通过`dispatchViewManagerCommand`来发送对应的指令，然后在 Java 层处理相应的逻辑，那我们怎么知道 JS 层到底发送了什么指令？其实就是在这个方法中来接收 JS 层发出的指令。
* `addEventEmitters`：重写此方法给给定的视图安装自定义事件发射器。如果这个视图需要发射除了基本的触摸事件以外的事件，那么就需要重写这个方法。这个例子中在开始刷新的回调方法中发送一个自定义的事件`ptrRefresh`给 JS 层，
* `getExportedCustomDirectEventTypeConstants`：返回 <u>传递给JS的</u> <u>定义可以放置在原生视图上的</u> <u>符合条件的事件</u> 的配置数据的 map 集合(定语比较多(╯‵□′)╯︵┻━┻)， 这应该返回非冒泡直接调度的事件类型。反正我是没看懂。。。以下是个人理解，我们注册了onPtrRefresh事件，这个事件是给JS 层使用的，JS 层在onPtrRefresh中执行刷新的逻辑（请求网络数据等），然后回调给 Java 层。正常NativeApp需要在onRefreshBegin里执行刷新逻辑， 使用 RN 就可以这样在 JS 层上执行刷新逻辑了。JS 层 onPtrRefresh --> Java 层 `ptrRefresh` --> `onRefreshBegin`。
* `addView`：为什么要重写 `addView` 呢？如果看过`PtrFrameLayout`源码就知道，是在`onFinishInflate`中 `findViewById`并初始化`contentView`和`headerView`的，可是 RN 根本不走`onFinishInflate`方法，因为`onFinishInflate`只是在从 `XML` 加载完成布局之后才调用（PS:最初我在`createViewInstance`方法中是`inflate` `XML`布局的，打Log是走`onFinishInflate`的。。。都是泪😭）。并且在调用`addView`的时候，`PtrClassicFrameLayout`中已经存在`headerView`了，在`PtrClassicFrameLayout`的构造方法中添加的。然后调用父类`addView`的时候，是从 Index 为0开始添加的，很显然逻辑就不对了，我们要把`onFinishInflate`中的代码稍微修改一下并且抽出来写成公有方法`finishInflateRN`，供外部调用。然后我们重写addView 把contentView 添加在 Index 为1的位置，并且调用一下`finishInflateRN`就好了。

最后就是暴露属性，让 JS 层可以使用这些属性，方法使用`ReactProp`注解，`name`就是 JS 的属性，还可以设置一些默认值。

### 代码片段

```java
public class ReactPtrAndroidManager extends ViewGroupManager<PtrClassicFrameLayout> {

    private static final int REFRESH_COMPLETE = 0;
    private static final int AUTO_REFRESH = 1;

    @Override
    public String getName() {
        return "RCTPtrAndroid";
    }

    @Override
    protected PtrClassicFrameLayout createViewInstance(final ThemedReactContext reactContext) {
        PtrClassicFrameLayout layout = new PtrClassicFrameLayout(reactContext);
        layout.setLayoutParams(new FrameLayout.LayoutParams(ViewGroup.LayoutParams.MATCH_PARENT, ViewGroup.LayoutParams.MATCH_PARENT));
        return layout;
    }

    @ReactProp(name = "resistance", defaultFloat = 1.7f)
    public void setResistance(PtrClassicFrameLayout ptr, float resistance) {
        ptr.setResistance(resistance);
    }

    @ReactProp(name = "durationToCloseHeader", defaultInt = 200)
    public void setDurationToCloseHeader(PtrClassicFrameLayout ptr, int duration) {
        ptr.setDurationToCloseHeader(duration);
    }

    @ReactProp(name = "durationToClose", defaultInt = 300)
    public void setDurationToClose(PtrClassicFrameLayout ptr, int duration) {
        ptr.setDurationToClose(duration);
    }

    @ReactProp(name = "ratioOfHeaderHeightToRefresh", defaultFloat = 1.2f)
    public void setRatioOfHeaderHeightToRefresh(PtrClassicFrameLayout ptr, float ratio) {
        ptr.setRatioOfHeaderHeightToRefresh(ratio);
    }

    @ReactProp(name = "pullToRefresh", defaultBoolean = false)
    public void setPullToRefresh(PtrClassicFrameLayout ptr, boolean pullToRefresh) {
        ptr.setPullToRefresh(pullToRefresh);
    }

    @ReactProp(name = "keepHeaderWhenRefresh", defaultBoolean = false)
    public void setKeepHeaderWhenRefresh(PtrClassicFrameLayout ptr, boolean keep) {
        ptr.setKeepHeaderWhenRefresh(keep);
    }

    @ReactProp(name = "pinContent", defaultBoolean = false)
    public void setPinContent(PtrClassicFrameLayout ptr, boolean pinContent) {
        ptr.setPinContent(pinContent);
    }


    @Nullable
    @Override
    public Map<String, Integer> getCommandsMap() {
        return MapBuilder.of("autoRefresh", AUTO_REFRESH, "refreshComplete", REFRESH_COMPLETE);
    }

    @Override
    public void receiveCommand(PtrClassicFrameLayout root, int commandId, @Nullable ReadableArray args) {
        switch (commandId) {
            case AUTO_REFRESH:
                root.autoRefresh();
                break;
            case REFRESH_COMPLETE:
                root.refreshComplete();
                break;
            default:
                break;
        }
    }

    @Override
    protected void addEventEmitters(final ThemedReactContext reactContext, final PtrClassicFrameLayout view) {
        view.setLastUpdateTimeRelateObject(this);
        view.setPtrHandler(new PtrDefaultHandler() {
            @Override
            public boolean checkCanDoRefresh(PtrFrameLayout frame, View content, View header) {
                return checkContentCanBePulledDown(frame, content, header);
            }

            @Override
            public void onRefreshBegin(PtrFrameLayout frame) {
                reactContext
                        .getNativeModule(UIManagerModule.class)
                        .getEventDispatcher()
                        .dispatchEvent(new PtrEvent(view.getId()));
            }
        });
    }

    @Nullable
    @Override
    public Map<String, Object> getExportedCustomDirectEventTypeConstants() {
        return MapBuilder.<String, Object>of(
                "ptrRefresh", MapBuilder.of("registrationName", "onRefresh"));
    }

    @Override
    public void addView(PtrClassicFrameLayout parent, View child, int index) {
        super.addView(parent, child, 1);
        parent.finishInflateRN();
    }
}
```

```java
public class PtrEvent extends Event<PtrEvent> {

    public PtrEvent(int viewTag) {
        super(viewTag);
    }

    @Override
    public String getEventName() {
        return "ptrRefresh";
    }

    @Override
    public void dispatch(RCTEventEmitter rctEventEmitter) {
        rctEventEmitter.receiveEvent(getViewTag(), getEventName(), null);
    }
}
```

```javascript
import React, {
    Component,
    PropTypes,
} from 'react';
import {
    requireNativeComponent,
    View,
} from 'react-native';
const UIManager = require('UIManager');
const ReactNative = require('ReactNative');
const REF_PTR = "ptr_ref";

export default class PtrComponent extends Component {
    constructor(props) {
        super(props);
        this._onRefresh = this._onRefresh.bind(this);
    }

    _onRefresh() {
        if (!this.props.handleRefresh) {
            return;
        }
        this.props.handleRefresh();
    };

    refreshComplete() {
        UIManager.dispatchViewManagerCommand(
            ReactNative.findNodeHandle(this.refs[REF_PTR]),
            0,
            null
        );
    }

    autoRefresh() {
        let self = this;
        UIManager.dispatchViewManagerCommand(
            ReactNative.findNodeHandle(self.refs[REF_PTR]),
            1,
            null
        );
    }

    render() {
        // onRefresh 事件对应原生的ptrRefresh事件
        return (
             <RCTPtrAndroid
                ref={REF_PTR}
                {...this.props}
                onRefresh={() => this._onRefresh()}/>
        );
    }
}

PtrComponent.name = "RCTPtrAndroid"; //便于调试时显示(可以设置为任意字符串)
PtrComponent.propTypes = {
    handleRefresh: PropTypes.func,
    resistance: PropTypes.number,
    durationToCloseHeader: PropTypes.number,
    durationToClose: PropTypes.number,
    ratioOfHeaderHeightToRefresh: PropTypes.number,
    pullToRefresh: PropTypes.bool,
    keepHeaderWhenRefresh: PropTypes.bool,
    pinContent: PropTypes.bool,
    ...View.propTypes,
};

const RCTPtrAndroid = requireNativeComponent('RCTPtrAndroid', PtrComponent, {nativeOnly: {onRefresh: true}});
```

然后我们就可以使用`PtrComponent`了：

```javascript
import React, {
    Component,
    PropTypes,
} from 'react';
import {
    Text,
    View,
    ToastAndroid,
    ScrollView,
} from 'react-native';
import PtrFrame from './component/PtrComponent';

export default class Search extends Component {
    static propTypes = {
        navigator: PropTypes.object,
    };

    render() {
        return (
            <PtrFrame
                ref='ptr'
                handleRefresh={() => this._getData()}
                durationToCloseHeader={300}
                durationToClose={200}
                resistance={2}
                pinContent={false}
                ratioOfHeaderHeightToRefresh={1.2}
                pullToRefresh={false}
                keepHeaderWhenRefresh={true}
                style={{flex: 1}}>
                <ScrollView style={{flex: 1, flexDirection: 'column'}}>
                    <View style={{backgroundColor: '#FFAA55', height: 200}}/>
                    <View style={{backgroundColor: '#FFAAAA', height: 200}}/>
                    <View style={{backgroundColor: '#FFAAFF', height: 200}}/>
                    <View style={{backgroundColor: '#00AAAA', height: 200}}/>
                    <View style={{backgroundColor: '#00AA99', height: 200}}/>
                    <View style={{backgroundColor: '#FF99AA', height: 200}}/>
                </ScrollView>
            </PtrFrame>
        );
    };

    _getData() {
      ToastAndroid.show("refreshing", ToastAndroid.SHORT);
        this.timer = setTimeout(() => {
            this.refs.ptr.refreshComplete();
        }, 2500);
    };

    componentWillUnmount() {
        this.timer && clearTimeout(this.timer);
    }
}
```

### 效果图

![](screen.gif)

### 项目地址

https://github.com/panda912/RNAndroidPullToRefresh

