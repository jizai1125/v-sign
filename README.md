

# v-sign  手写签名

**如有问题或者建议，欢迎留言或加群联系我（群号：736123963）！！！将保持维护！！！**

<img src="https://i.loli.net/2021/12/02/bgsfnDmCzXGq8ct.png" alt="uniapp 交流群群聊二维码" style="zoom: 80%;" />

## 快速使用

基础：

```vue
<v-sign></v-sign>
```

使用 `v-sign-controls` 按钮子组件

```vue
<template>
    <v-sign>
		<v-sign-controls></v-sign-controls>
	</v-sign>
</template>
<script>
    import vSignControl from '@/components/v-sign/v-sign-control'
    export default {
        components: {
			vSignControl
		}
    }
</script>
```

## API

### 属性 (Props)

|   属性名    |     类型      |    默认值     |             说明             |
| :---------: | :-----------: | :-----------: | :--------------------------: |
|     cid     |    String     | v-sign-时间戳 |          canvas id           |
|    width    | String/Number |     100%      | canvas 宽度，Number 单位 rpx |
|   height    | String/Number |    300rpx     | canvas 高度，Number 单位 rpx |
| customStyle |    Object     |       -       |      canvas 自定义样式       |
|  lineWidth  |    Number     |       4       |             线宽             |
|  lineColor  |    String     |     #000      |            线颜色            |

### 事件（Events）

| 事件称名 |                             说明                             |               返回值               |
| :------: | :----------------------------------------------------------: | :--------------------------------: |
|  @init   | 创建完 canvas 实例后触发，向外提供 canvas实例，撤回，清空方法 | Object：具体见下方事件回调参数说明 |

## 事件回调参数说明

### init

如果不使用提供 [v-sign-controls](#v-sign-controls) 子组件操作画布，则可以通过该事件回调暴露的 clear、revoke 等方法。

```js
{
    // canvas 实例
	ctx: Object,
	// 清空画布
	clear: Function,
	// 撤回
	revoke: Function,
    // 返回为图片临时文件路径，用法同 uni.canvasToTempFilePath方法，内部只是做了 Promise 化处理而已
    canvasToTempFilePath: <Promise>Function
}
```

示例：

```html
<template>
	<v-sign @init="onSignInit"></v-sign>
	<button @click="clear">清空<button>
	<button @click="revoke">撤回<button>
    <button @click="saveTempFile">保存<button>
</template>
<script>
    export default {
        methods: {
            onSignInit(signCtx) {
                this.signCtx = signCtx
            },
            // 清空
            clear() {
                this.signCtx.clear()
            },
            // 撤回
			revoke() {
                this.signCtx.revoke()
            },
            // 保存
            saveTempFile() {
                this.signCtx.canvasToTempFilePath({fileType: 'jpg'})
                  	.then(filePath => {
                    	console.log(filePath)
	                })
            }
        }
    }
</script>
```

# 子组件

## 按钮控件（v-controls）

### API

### 属性 (Props)

|   属性名    |     类型      |          默认值           |                          说明                          |
| :---------: | :-----------: | :-----------------------: | :----------------------------------------------------: |
|   actions   |     Array     | ["clear", "prev", "save"] | 按钮配置；清空（clear）, 撤回（prev） 保存图片（save） |
|   border    |    Boolean    |           true            |                     按钮是否有边框                     |
|    space    | String/Number |          300rpx           |               按钮间隔，Number 单位 rpx                |
| customStyle |    Object     |             -             |                    根元素自定义样式                    |

### 事件（Events）

点击对应类型按钮触发对应事件， 例如点击 清空（clear）按钮则触发 clear 事件

示例：

```html
<template>
    <v-sign>
		<v-sign-controls @save="save"></v-sign-controls>
	</v-sign>
</template>
<script>
    import vSignControl from '@/components/v-sign/v-sign-control'
    export default {
        components: {
			vSignControl
		},
        methods: {
            save(tempFilePath) {
                console.log(tempFilePath);
            }
		}
    }
</script>
```
