---
typora-root-url: ..\..
categories: 
    - react
tags: 
    - react
    - 小程序
    - 评分
cover: /static/images/20200709173603.png
---

### 效果图

![](/static/images/20200709173603.png)

### StarScore.tsx

```react
import Taro from "@tarojs/taro"
import { View } from "@tarojs/components"
import ImageIcon from "~/components/Image/Icon"
import "./StarScore.less"

interface Props {
  /**
   * 分数
   */
  score: number

  /**
   * 分数改变事件
   */
  onChange?: (score: number) => void

  /**
   * 图标大小
   */
  size?: number

  /**
   * 最多几颗星星
   */
  max?: number

  /**
   * 能不能点击
   */
  complete?: boolean

  /**
   * 是否显示tip
   */
  showTip?: boolean

  /**
   * tip数组，长度与max对应
   */
  tips?: Array<string>
}

const MAX = 5
export default class StarScore extends Taro.Component<Props> {
  static defaultProps = {
    complete: true
  }

  static externalClasses = ["item-class", "container-class"]

  onChange(score: number) {
    if (this.props.onChange && !this.props.complete) this.props.onChange(score)
  }

  render() {
    let { score } = this.props
    const {
      size = 16,
      max = MAX,
      tips = ["非常差", "差", "一般", "好", "非常好"],
      showTip = false
    } = this.props
    score = Math.floor(score)
    return (
      <View className="container container-class">
        {new Array(max).fill(0).map((_item: number, key: number) => {
          return (
            <View
              key={`score-${key}`}
              className="item item-class"
              onClick={this.onChange.bind(this, key + 1)}
            >
              {score >= key + 1 ? (
                <ImageIcon
                  data-key={key + 1}
                  data-score={score}
                  name="StarChecked"
                  size={size}
                />
              ) : (
                <ImageIcon
                  data-key={key + 1}
                  data-score={score}
                  name="Star"
                  size={size}
                />
              )}
            </View>
          )
        })}
        {showTip && <View>{tips[score - 1]}</View>}
      </View>
    )
  }
}

```

### StarScore.less

```less
.container {
  height: 24px;

  .item {
    margin-right: 7px;
    display: inline-block;

    &:last-child {
      margin-right: 0;
    }
  }
}

```

### 使用

```react
import Taro from "@tarojs/taro"
import { View } from "@tarojs/components"
import StarScore from "~/components/StarScore/StarScore"

interface Props {}

export default class Example extends Taro.Component<Props> {
  config: Taro.Config = {
    navigationBarTitleText: "测试页面"
  }

  state = {
    score: 4
  }
  render() {
    const { score } = this.state
    return (
      <View>
        <StarScore
          score={score}
          showTip
          complete={false}
          size={25}
          onChange={val => {
            this.setState({
              score: val
            })
          }}
        />
      </View>
    )
  }
}

```

