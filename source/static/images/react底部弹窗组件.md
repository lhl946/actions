---
typora-root-url: ..
---

### 效果图

![](/images/20200709141237.png)

### Popup.tsx

```react
import Taro, { Component } from "@tarojs/taro"
import { View } from "@tarojs/components"
import "./Popup.less"

interface IProps {
  onClose?: (e) => {}
  title?: string
  closeByOverlay?: boolean
}

export default class Popup extends Component<IProps, {}> {
  static defaultProps = {
    closeByOverlay: true
  }
  state = {
    isOpened: false
  }

  // 点击蒙层
  handleClose() {
    // 允许蒙层关闭
    this.props.closeByOverlay && this.close()

    // 有监听onclose方法
    this.props.onClose && this.props.onClose(false)
  }

  // 打开
  open() {
    this.setState({
      isOpened: true
    })
  }

  // 关闭
  close() {
    this.setState({
      isOpened: false
    })
  }

  render() {
    const { title } = this.props
    const { isOpened } = this.state
    return (
      <View className={isOpened ? "flolayout active" : "flolayout"}>
        <View
          className="flolayout__overlay"
          onClick={this.handleClose.bind(this, isOpened)}
        ></View>
        <View className="flolayout__container layout">
          <View className="layout-header">{title}</View>
          <View className="layout-body">{this.props.children}</View>
        </View>
      </View>
    )
  }
}

```

### popup.less

```react
.flolayout {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  visibility: hidden;
  z-index: 810;
  transition: visibility 300ms cubic-bezier(0.36, 0.66, 0.04, 1);
  &.active {
    visibility: visible;
    .flolayout__overlay {
      opacity: 1;
    }
    .flolayout__container {
      transform: translate3d(0, 0, 0);
    }
  }
}
.flolayout__overlay {
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  position: absolute;
  background-color: rgba(0, 0, 0, 0.3);
  opacity: 0;
  transition: opacity 150ms ease-in;
}
.flolayout__container {
  position: absolute;
  bottom: 0;
  width: 100%;
  // min-height: 600px;
  // max-height: 950px;
  background-color: #fff;
  border-radius: 32px 32px 0px 0px;
  transform: translate3d(0, 100%, 0);
  transition: -webkit-transform 300ms cubic-bezier(0.36, 0.66, 0.04, 1);
  transition: transform 300ms cubic-bezier(0.36, 0.66, 0.04, 1);
  transition: transform 300ms cubic-bezier(0.36, 0.66, 0.04, 1),
    -webkit-transform 300ms cubic-bezier(0.36, 0.66, 0.04, 1);
}
.layout {
  display: block;
  background-color: #fff;
}

.flolayout .layout-header {
  position: relative;
  padding: 30px 0;
  text-align: center;
  .close-img {
    position: absolute;
    right: 28px;
    top: 36px;
    width: 36px;
    height: 36px;
  }
}
.flolayout .layout-header__title {
  overflow: hidden;
  -o-text-overflow: ellipsis;
  text-overflow: ellipsis;
  white-space: nowrap;
  color: #333;
  font-size: 32px;
  display: block;
  padding-right: 80px;
}
.flolayout .layout-header__icon {
  line-height: 1;
  position: absolute;
  top: 50%;
  right: 18px;
  padding: 10px;
  transform: translate(0, -50%);
}

.flolayout .layout-body {
  font-size: 28px;
  padding: 20px;
  // height: 602px;
}
.flolayout .layout-body__content {
  position: relative;
  height: 500px;
  overflow-y: scroll;
}

```

### 使用

```
import Taro from "@tarojs/taro"
import { View } from "@tarojs/components"
import Popup from "~/components/Popup/Popup"

export default class Example extends Taro.Component {
  config: Taro.Config = {
    navigationBarTitleText: "测试页面"
  }

  popup: Popup

  render() {
    return (
      <View>
        <View
          onClick={() => {
            this.popup.open()
          }}
        >
          展开
        </View>
        <Popup
          title="标题"
          ref={e => {
            if (e) this.popup = e
          }}
        >
          <View>传任意内容</View>
          <View>传任意内容</View>
          <View>传任意内容</View>
          <View>传任意内容</View>
        </Popup>
      </View>
    )
  }
}

```

