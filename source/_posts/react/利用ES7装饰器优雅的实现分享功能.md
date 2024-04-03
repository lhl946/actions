利用ES7装饰器优雅的实现分享功能

### 一、场景

小程序分享功能，由于有些页面是需要，有些页面是不需要，不能全局配置。所以我只能在需要分享功能的页面一遍又一遍的复制如下代码。

```react
//分享
onShareAppMessage() {
    return {
        title: "来自xxx的分享",
        path: `/pages/index/index`,
        imageUrl: 'https://www.baidu.com/logo.png'
    }
}
```

### 二、思路

一次偶然的机会，突然想起来，ES7有个叫装饰器的东西，为什么不试一下呢。

1、将onShareAppMessage这个函数抽出来。

2、利用装饰器直接把onShareAppMessage注入到类的原型上。

### 三、具体步骤

decorator.ts

```react
import Taro, { ComponentClass } from '@tarojs/taro'

// 分享功能装饰器
export function share(target: ComponentClass) {
    Object.defineProperty(target.prototype, 'onShareAppMessage', {
        value() {
            return {
                title: "来自xx的分享",
                path: `/pages/index/index`,
                imageUrl: 'https://www.baidu.com/logo.png'
            }
        }
    })
}
```

页面上使用

```react
import Taro from "@tarojs/taro"
import { View } from "@tarojs/components"
import "./test.less"
import { share } from '~/utils/decorator'

interface Props { }

interface State { }

@share
export default class Example extends Taro.Component<Props, State> {
  config: Taro.Config = {
    navigationBarTitleText: "测试页面"
  }

  render() {
    return (
      <View>
      </View>
    )
  }
}

```

### 四、说明

新建页面的时候，如果需要分享功能，直接在类上面写一个“@share”就行了（当然是你已经import的情况下）。是不是比以前优雅了很多呢。

