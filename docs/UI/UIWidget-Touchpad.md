# UI 控件-摄像机滑动区

**阅读本文大概需要 5 分钟**

本文概述了 UI 控件—摄像机滑动区的各项属性以及使用方法。

## 什么是摄像机滑动区？

**摄像机滑动区**是一种不可见的交互区域，在摄像机滑动区内可以通过滑动屏幕的方式，控制摄像机镜头转向。

- 变换/对齐/通用/渲染属性请见 [UI 控件的基础属性](https://docs.ark.online/UI/UIWidget-BaseProperties.html)
- 请注意：为了确保使用大面积摄像机滑动区的游戏中，能正常触发下层UI的touch事件以及与世界UI交互，摄像机滑动区空间的交互优先级永远最低，例如摄像机滑动区下方放置的按钮也可以被点击到。

## 摄像机滑动区属性


### 灵敏度比例

- 功能说明：此属性数值越大，摄像机滑动区的灵敏度越高

### 是否被鼠标控制

- 功能说明：设置此摄像机滑动区是否允许被鼠标控制，此属性只作用于 PC 端，不会影响移动端
- 在为 PC 游戏端玩家设计控制方案时，为了避免按键绑定与直接点击 UI 相互冲突，需要合理的运用此项属性，例如：

  - 射击游戏中通常使用鼠标左键作为开火键
  - 而编辑器的默认逻辑是直接点击 UI 的优先级高于按键绑定，也就是说如果摄像机滑动区控件在屏幕中占据较大范围，鼠标左键点击在摄像机滑动区控件范围内都将优先操控摄像机滑动区，而不会触发开火
  - 因此针对 PC 端玩家，推荐摄像机滑动区不勾选是否被鼠标控制，而是通过按键绑定或者鼠标锁定功能来控制视角；如果出于测试目的需要能使用鼠标操控此摄像机滑动区，则需要勾选是否被鼠标控制
- 更多关于键鼠按键绑定的介绍请见[按键绑定（针对 PC 端）及预设 UI](https://docs.ark.online/UI/KeybindsandPremadeUI.html)
- 示意图：

## 如何使用摄像机滑动区？

### 示例一：制作动态调节灵敏度的摄像机滑动区

```ts
@UIBind('')
export default class DefaultUI extends UIScript {
	private character: Character;

	/** 仅在游戏时间对非模板实例调用一次 */
    protected onStart() { 
		//设置能否每帧触发onUpdate
		this.canUpdate = false;
		
		let Plus = this.uiWidgetBase.findChildByPath('MWCanvas/StaleButton') as StaleButton
		let Inc = this.uiWidgetBase.findChildByPath('MWCanvas/StaleButton_1') as StaleButton
		let TouchPad = this.uiWidgetBase.findChildByPath('MWCanvas/TouchPad') as TouchPad
		let text=this.uiWidgetBase.findChildByPath('MWCanvas/TextBlock') as TextBlock
	
		Plus.onPressed.add(()=>{ 
			TouchPad.inputScale=(new Vector2(1,1))
			let num =TouchPad.inputScale
			text.text=(num.toString())
		})
	
		Inc.onPressed.add(()=>{ 
			TouchPad.inputScale=(new Vector2(0.2,0.2))
			let num =TouchPad.inputScale
			text.text=(num.toString())
		})  
  }
}
```

- 示意视频：
<video controls src="https://qn-cdn.233leyuan.com/online/mzQpKKbZ3Gyv1724134721326.mp4"></video>


### 示例二：动态调整摄像机滑动区是否可用

- 使摄像机滑动区不可用有两种方法

  - 调整可用性为不可用

```ts
let touchPad = this.uiWidgetBase.findChildByPath('Canvas/TouchPad') as TouchPad
//调整摄像机滑动区的可用性为不可用
touchPad.isEnabled=false
//调整摄像机滑动区的可用性为可用
touchPad.isEnabled=true
```

- 调整可见性为隐藏/折叠/可见不可交互也都可以使摄像机滑动区不可使用

```ts
let touchPad = this.uiWidgetBase.findChildByPath('Canvas/TouchPad') as TouchPad
//调整摄像机滑动区的可见性为折叠，1234这四种可见性都会使摄像机滑动区不可使用
touchPad.visibility=1
//调整摄像机滑动区的可见性为可见，即可以使用
touchPad.visibility=0
```

- 在属性面板中也可以调整摄像机滑动区的可见性和可用性

![](https://qn-cdn.233leyuan.com/online/K9sHbmwpe8Ef1724134722645.png)
