## form-generator的uniapp解析器

> 用于将form-generator导出的JSON解析成一个表单。

### 依赖

> 依赖于uview1.8组件库

### 安装组件

```
npm i uniapp-form-generator-parser
```

或者

```
yarn add uniapp-form-generator-parser
```

### 使用示例

> [查看在线示例](https://mrhj.gitee.io/form-generator/#/parser)

示例代码:

> [example/index.vue](https://gitee.com/time-doesnt-wait-for-me/uniapp-form-generator-parser/blob/master/example/index.vue)

### 注意

因为uview表单验证number类型不通过验证，故修改了uview的一点源码组件（其实就是把组件$emit事件的值转化为String字符串类型）如下：

计数器组件：u-number-box 搜索 

原来为：

```javascript
this.emit('input',Number(value));
value:Number(value)
```

修改为：

```javascript
this.emit('input', String(value));
value: String(value)
```



评分组件：u-rate 搜索 

原来为： 

```javascript
this.emit('change',this.activeIndex)
this.emit('change',this.activeIndex)
this.emit('input', this.activeIndex) 
```

修改为： 

```javascript
this.emit(′change′,String(this.activeIndex))
this.emit(′change′,String(this.activeIndex))
this.emit('input', String(this.activeIndex))
```

上传组件：u-upload 搜索 原来为：

this.showToast('超出允许的文件大小');

修改为： this.showToast(`超出允许的文件大小${this.maxSize / 1024 /1024}M`);



表单组件：u-form-item搜索validation方法

在

```javascript
// 检验之间，先获取需要校验的值
this.fieldValue = this.parent.model[this.prop];
```

之后加入

```javascript
if(Array.isArray(this.fieldValue)){//如果是数组，转成字符串型通过验证
	this.fieldValue = this.fieldValue.toString();
}
```

备注： 如果报错误TypeError: Cannot read property 'model' of undefined 尝试修改组件oFormParser里面setTimeout 延迟时间加长。

### 鸣谢

源代码作者[Mr.Liang](https://gitee.com/liang1022)

点击[地址](https://gitee.com/liang1022/form-generator-parser/tree/master)跳转