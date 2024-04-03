---
title: 小程序input限制输入最大数问题
date: 2020-07-08 14:39:45
categories: 
    - react
tags: 
    - react
    - 小程序
    - input
cover: /static/images/20200708155238.png
typora-root-url: ..\..
---

# 小程序input限制输入最大数问题

需求：类似购物车点加减号，中间可以输入具体的数量。超过最大值之后，数字设置为最大值

![](/static/images/20200708155238.png)

## 上代码

```react
import Taro from "@tarojs/taro"
import { View, Input } from "@tarojs/components"
import MyIcon from "~/components/Image/Icon"
import "./Stepper.less"
import { values } from "mobx"

interface Props {
  value?: number
  max?: number
  min?: number
  step?: number
  onchange?: (e) => {}
}
interface State {
  scale: number
}

export default class Stepper extends Taro.Component<Props, State> {
  static defaultProps = {
    step: 1,
    value: 0
  }
  componentWillMount() {
    const value = this.props.value || 0
    this.setState({ scale: value })
  }
  componentDidMount() {}
  //点击加号或减号
  count(action: "add" | "reduce") {
    const { max, min, step } = this.props
    this.setState(({ scale }) => {
      if (action === "add") {
        if (step) scale += step
      } else {
        if (step) scale -= step
      }
      if (min && scale <= min) {
        scale = min
      }
      if (max && scale >= max) {
        scale = max
      }
      scale.toFixed(1)

      return { scale: parseFloat(scale.toFixed(1)) }
    })
  }

  testValue(val: string, min: number, max: number) {
    let value = parseInt(val)
    if (!value) {
      value = min || 0
      this.setState({
        scale: value
      })
    }

    if (min && value <= min) {
      value = min
    }
    if (max && value >= max) {
      value = max
    }
    this.setState({
      scale: value
    })
  }

  render() {
    const scale = this.state.scale || 0
    console.log(scale)
    const { max, min } = this.props
    return (
      <View className="container">
        <View
          className="counter-icon"
          onClick={this.count.bind(this, "reduce")}
        >
          <MyIcon name="icon_minus" />
        </View>
        <Input
          className="counter-input"
          type="number"
          value={scale.toString()}
          onInput={e => {
            this.testValue(e.detail.value, min, max)
          }}
        />
        <View className="counter-icon" onClick={this.count.bind(this, "add")}>
          <MyIcon name="icon_plus" />
        </View>
      </View>
    )
  }
}

```

## 引用

```react
<Stepper step={2} max={10} />
```

## 结果

点击加号能限制到10，但是输入的时候就不行了。

输入1000，显示的是1000，控制台打印scale的值是10；

输入2000，显示的是1000，控制台打印scale的值是10；

![20200708160538](/static/images/20200708160538.png)

## 后续

暂时是提交订单的时候用toast提示，然后把scale设置为10。以后进步了再回来修复这个问题