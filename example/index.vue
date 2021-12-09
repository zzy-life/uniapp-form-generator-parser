<template>
	<view>

		<o-form-parser v-if="parserisnot" :formParserData="formParserData" @eventSubmit="onSubmit"></o-form-parser>
		<!-- <u-select v-model="show" mode="single-column" :list="list" @confirm="confirm"></u-select> -->
	</view>
</template>

<script>
	import oFormParser from '../oFormParser';

	export default {
		data() {
			return {
				show: true,
				list: [{
						value: '1',
						label: '江'
					},
					{
						value: '2',
						label: '湖'
					}
				],
				// 表单json数据
				formParserData: [],
				parserisnot: false,
				// 标准格式数据
				formParserData2: [{
						keyName: "name",
						value: "",
						label: "用户名",
						tag: "input",
						type: "text",
						placeholder: "请输入用户名",
						required: true,
						regList: [],
						disabled: false,
					}, {
						keyName: "mobile",
						value: "",
						label: "手机号码",
						tag: "input",
						type: "text",
						placeholder: "请输入手机号码",
						required: true,
						regList: [{
							pattern: "^[1][3,4,5,7,8][0-9]{9}$",
							message: "手机号格式错误",
						}]
					}, {
						keyName: "remarks",
						value: "",
						label: "备注",
						tag: "input",
						type: "textarea",
						placeholder: "请输入备注",
						required: false,
						regList: [],
						disabled: false,
					},
					{
						keyName: "password",
						value: "",
						label: "密码",
						tag: "input",
						type: "password",
						placeholder: "请输入密码",
						required: true,
						regList: [],
						disabled: false,

					},
					{
						keyName: "number",
						value: "",
						label: "证件数",
						tag: "input-number",
						type: "input-number",
						placeholder: "请输入证件数",
						required: true,
						regList: [],
						disabled: false,
						min: 0,
						max: 10,
						step: 1,
					},
					{
						keyName: "star",
						value: "",
						label: "评分",
						tag: "rate",
						type: "rate",
						placeholder: "请选择评分",
						required: true,
						regList: [],
						disabled: false,
						max: 5,
						allowHalf: false,
					}, {
						keyName: "agree",
						value: true,
						label: "是否同意",
						tag: "switch",
						type: "switch",
						placeholder: "请选择是否同意",
						required: true,
						regList: [],
						disabled: false,
					}, {
						keyName: "sex",
						value: "1",
						label: "性别",
						tag: "radio",
						type: "radio",
						placeholder: "请选择性别",
						required: true,
						regList: [],
						disabled: false,
						options: [{
							label: "男",
							value: "1"
						}, {
							label: "女",
							value: "2"
						}, {
							label: "未知",
							value: "3"
						}]
					}, {
						keyName: "like",
						value: "2,3",
						label: "爱好",
						tag: "checkbox",
						type: "checkbox",
						placeholder: "请选择爱好",
						required: true,
						regList: [],
						disabled: false,
						options: [{
							label: "编程",
							value: "1",
							checked: false,
						}, {
							label: "音乐",
							value: "2",
							checked: true,
						}, {
							label: "电影",
							value: "3",
							checked: true,
						}, {
							label: "美食",
							value: "4",
							checked: false,
						}, {
							label: "旅游",
							value: "5",
							checked: false,
						}, {
							label: "睡觉",
							value: "6",
							checked: false,
						}]
					}, {
						keyName: "dates",
						value: "2021-06-19",
						label: "日期选择",
						tag: "picker-date",
						type: "select",
						placeholder: "日期选择",
						required: true,
						regList: [],
						disabled: false,
					}, {
						keyName: "times",
						value: "17:50:56",
						label: "时间选择",
						tag: "picker-time",
						type: "select",
						placeholder: "时间选择",
						required: true,
						regList: [],
						disabled: false,
					}, {
						keyName: "selectSingle",
						value: "",
						label: "单列下拉",
						tag: "select-single",
						type: "single-column",
						placeholder: "请选择单列下拉",
						required: true,
						regList: [],
						disabled: false,
						options: [{
							label: "选项一",
							value: "1"
						}, {
							label: "选项二",
							value: "2"
						}, {
							label: "选项三",
							value: "3"
						}]
					}, {
						keyName: "selectMutil",
						value: "",
						label: "级联下拉",
						tag: "select-mutil",
						type: "mutil-column-auto",
						placeholder: "请选择级联下拉",
						required: true,
						regList: [],
						disabled: false,
						options: [{
							label: "选项1",
							value: "1",
							children: [{
								label: "选项1-1",
								value: "2",
							}]
						}, {
							label: "选项2",
							value: "3",
							children: [{
								label: "选项2-1",
								value: "4",
							}, {
								label: "选项2-2",
								value: "5",
							}]
						}, ]
					}, {
						action: "https://jsonplaceholder.typicode.com/posts/",
						autoUpload: true,
						keyName: "card",
						label: "上传身份证",
						maxCount: 3,
						maxSize: 2,
						multiple: false,
						name: "file",
						placeholder: "请选择上传身份证",
						regList: [],
						required: true,
						tag: "upload-image",
						type: "image",
						value: null
					}
				]
			}
		},
		components: {
			oFormParser
		},
		created() {
			this.apiGetData();
		},
		methods: {
			// 模拟请求接口返回数据
			apiGetData() {
				let jsonData = {
					"fields": [{
						"__config__": {
							"label": "姓名",
							"labelWidth": null,
							"showLabel": true,
							"changeTag": true,
							"tag": "el-input",
							"tagIcon": "input",
							"required": true,
							"layout": "colFormItem",
							"span": 24,
							"document": "https://element.eleme.cn/#/zh-CN/component/input",
							"regList": [],
							"formId": 100,
							"renderKey": 1624354105602,
						},
						"__slot__": {
							"prepend": "",
							"append": ""
						},
						"placeholder": "请输入姓名",
						"style": {
							"width": "100%"
						},
						"clearable": true,
						"prefix-icon": "",
						"suffix-icon": "",
						"maxlength": null,
						"show-word-limit": false,
						"readonly": false,
						"disabled": false,
						"__vModel__": "field100"
					}, {
						"__config__": {
							"label": "姓名",
							"labelWidth": null,
							"showLabel": true,
							"changeTag": true,
							"tag": "el-input",
							"tagIcon": "input",
							"required": true,
							"layout": "colFormItem",
							"span": 24,
							"document": "https://element.eleme.cn/#/zh-CN/component/input",
							"regList": [],
							"formId": 101,
							"renderKey": 1624351505602,
						},
						"__slot__": {
							"prepend": "",
							"append": ""
						},
						"placeholder": "请输入姓名",
						"style": {
							"width": "100%"
						},
						"clearable": true,
						"prefix-icon": "",
						"suffix-icon": "",
						"maxlength": null,
						"show-word-limit": false,
						"readonly": false,
						"disabled": false,
						"__vModel__": "field101"
					}, {
						"__config__": {
							"label": "电话号码",
							"labelWidth": null,
							"showLabel": true,
							"changeTag": true,
							"tag": "el-input",
							"tagIcon": "input",
							"required": true,
							"layout": "colFormItem",
							"span": 24,
							"document": "https://element.eleme.cn/#/zh-CN/component/input",
							"regList": [{
								"pattern": "^[1][3,4,5,7,8][0-9]{9}$",
								"message": "电话格式错误"
							}],
							"formId": 103,
							"renderKey": 1624351523866
						},
						"__slot__": {
							"prepend": "",
							"append": ""
						},
						"placeholder": "请输入电话号码",
						"style": {
							"width": "100%"
						},
						"clearable": true,
						"prefix-icon": "",
						"suffix-icon": "",
						"maxlength": null,
						"show-word-limit": false,
						"readonly": false,
						"disabled": false,
						"__vModel__": "field103"
					}, {
						"__config__": {
							"label": "设置密码",
							"showLabel": true,
							"labelWidth": null,
							"changeTag": true,
							"tag": "el-input",
							"tagIcon": "password",
							"defaultValue": "",
							"layout": "colFormItem",
							"span": 24,
							"required": true,
							"regList": [],
							"document": "https://element.eleme.cn/#/zh-CN/component/input",
							"formId": 107,
							"renderKey": 1624351785826
						},
						"__slot__": {
							"prepend": "",
							"append": ""
						},
						"placeholder": "请输入设置密码",
						"show-password": true,
						"style": {
							"width": "100%"
						},
						"clearable": true,
						"prefix-icon": "",
						"suffix-icon": "",
						"maxlength": null,
						"show-word-limit": false,
						"readonly": false,
						"disabled": false,
						"__vModel__": "field107"
					}, {
						"__config__": {
							"label": "个人简介",
							"labelWidth": null,
							"showLabel": true,
							"tag": "el-input",
							"tagIcon": "textarea",
							"required": false,
							"layout": "colFormItem",
							"span": 24,
							"regList": [],
							"changeTag": true,
							"document": "https://element.eleme.cn/#/zh-CN/component/input",
							"formId": 108,
							"renderKey": 1624351814586
						},
						"type": "textarea",
						"placeholder": "请输入个人简介",
						"autosize": {
							"minRows": 4,
							"maxRows": 4
						},
						"style": {
							"width": "100%"
						},
						"maxlength": null,
						"show-word-limit": false,
						"readonly": false,
						"disabled": false,
						"__vModel__": "field108"
					}, {
						"__config__": {
							"label": "性别",
							"labelWidth": null,
							"showLabel": true,
							"tag": "el-radio-group",
							"tagIcon": "radio",
							"changeTag": true,
							"defaultValue": 1,
							"layout": "colFormItem",
							"span": 24,
							"optionType": "default",
							"regList": [],
							"required": true,
							"border": false,
							"document": "https://element.eleme.cn/#/zh-CN/component/radio",
							"formId": 104,
							"renderKey": 1624351622011
						},
						"__slot__": {
							"options": [{
								"label": "男",
								"value": 1
							}, {
								"label": "女",
								"value": 2
							}, {
								"label": "未知",
								"value": 3
							}]
						},
						"style": {},
						"size": "medium",
						"disabled": false,
						"__vModel__": "field104"
					}, {
						"__config__": {
							"label": "出生年月",
							"tag": "el-date-picker",
							"tagIcon": "date",
							"defaultValue": null,
							"showLabel": true,
							"labelWidth": null,
							"span": 24,
							"layout": "colFormItem",
							"required": true,
							"regList": [],
							"changeTag": true,
							"document": "https://element.eleme.cn/#/zh-CN/component/date-picker",
							"formId": 105,
							"renderKey": 1624351659418
						},
						"placeholder": "请选择出生年月",
						"type": "date",
						"style": {
							"width": "100%"
						},
						"disabled": false,
						"clearable": true,
						"format": "yyyy-MM-dd",
						"value-format": "yyyy-MM-dd",
						"readonly": false,
						"__vModel__": "field105"
					}, {
						"__config__": {
							"label": "出生时间",
							"tag": "el-time-picker",
							"tagIcon": "time",
							"defaultValue": null,
							"span": 24,
							"showLabel": true,
							"layout": "colFormItem",
							"labelWidth": null,
							"required": true,
							"regList": [],
							"changeTag": true,
							"document": "https://element.eleme.cn/#/zh-CN/component/time-picker",
							"formId": 118,
							"renderKey": 1624435591698
						},
						"placeholder": "请选择出生时间",
						"style": {
							"width": "100%"
						},
						"disabled": false,
						"clearable": true,
						"picker-options": {
							"selectableRange": "00:00:00-23:59:59"
						},
						"format": "HH:mm:ss",
						"value-format": "HH:mm:ss",
						"__vModel__": "field118"
					}, {
						"__config__": {
							"label": "反馈评价",
							"tag": "el-rate",
							"tagIcon": "rate",
							"defaultValue": 0,
							"span": 24,
							"showLabel": true,
							"labelWidth": null,
							"layout": "colFormItem",
							"required": true,
							"regList": [],
							"changeTag": true,
							"document": "https://element.eleme.cn/#/zh-CN/component/rate",
							"formId": 106,
							"renderKey": 1624351729258
						},
						"style": {},
						"max": 5,
						"allow-half": false,
						"show-text": false,
						"show-score": false,
						"disabled": false,
						"__vModel__": "field106"
					}, {
						"__config__": {
							"label": "户口人数",
							"showLabel": true,
							"changeTag": true,
							"labelWidth": null,
							"tag": "el-input-number",
							"tagIcon": "number",
							"defaultValue": 1,
							"span": 24,
							"layout": "colFormItem",
							"required": true,
							"regList": [],
							"document": "https://element.eleme.cn/#/zh-CN/component/input-number",
							"formId": 110,
							"renderKey": 1624351921259
						},
						"placeholder": "户口人数",
						"step": 1,
						"step-strictly": false,
						"controls-position": "",
						"disabled": false,
						"__vModel__": "field110",
					}, {
						"__config__": {
							"label": "下拉选择",
							"showLabel": true,
							"labelWidth": null,
							"tag": "el-select",
							"tagIcon": "select",
							"layout": "colFormItem",
							"span": 24,
							"required": true,
							"regList": [],
							"changeTag": true,
							"document": "https://element.eleme.cn/#/zh-CN/component/select",
							"formId": 158,
							"renderKey": "1011638952074933"
						},
						"__slot__": {
							"options": [{
								"label": "选项一",
								"value": 1
							}, {
								"label": "选项二",
								"value": 2
							}]
						},
						"placeholder": "请选择下拉选择",
						"style": {
							"width": "100%"
						},
						"clearable": true,
						"disabled": false,
						"filterable": false,
						"multiple": false,
						"__vModel__": "field158"
					}, {
						"__config__": {
							"label": "户口地址",
							"url": "https://www.fastmock.site/mock/f8d7a54fb1e60561e2f720d5a810009d/fg/cascaderList",
							"method": "get",
							"dataKey": "list",
							"showLabel": true,
							"labelWidth": null,
							"tag": "el-cascader",
							"tagIcon": "cascader",
							"layout": "colFormItem",
							"defaultValue": [],
							"dataType": "static",
							"span": 24,
							"required": true,
							"regList": [],
							"changeTag": true,
							"document": "https://element.eleme.cn/#/zh-CN/component/cascader",
							"formId": 115,
							"renderKey": 1624429110816
						},
						"options": [{
							"label": "四川省",
							"value": 1,
							"id": 103,
							"children": [{
								"label": "成都市",
								"value": 2,
								"id": 104
							}, {
								"label": "遂宁市",
								"value": 3,
								"id": 105
							}]
						}, {
							"label": "广东省",
							"value": 4,
							"id": 106,
							"children": [{
								"label": "深圳市",
								"value": 5,
								"id": 107
							}, {
								"label": "珠海市",
								"value": 6,
								"id": 108
							}]
						}],
						"placeholder": "请选择户口地址",
						"style": {
							"width": "100%"
						},
						"props": {
							"props": {
								"multiple": false,
								"label": "label",
								"value": "value",
								"children": "children"
							}
						},
						"show-all-levels": true,
						"disabled": false,
						"clearable": true,
						"filterable": false,
						"separator": "/",
						"__vModel__": "field115"
					}, {
						"__config__": {
							"label": "兴趣爱好",
							"tag": "el-checkbox-group",
							"tagIcon": "checkbox",
							"defaultValue": [],
							"span": 24,
							"showLabel": true,
							"labelWidth": null,
							"layout": "colFormItem",
							"optionType": "default",
							"required": true,
							"regList": [],
							"changeTag": true,
							"border": false,
							"document": "https://element.eleme.cn/#/zh-CN/component/checkbox",
							"formId": 112,
							"renderKey": 1624352073387
						},
						"__slot__": {
							"options": [{
								"label": "编程",
								"value": 1
							}, {
								"label": "旅行",
								"value": 2
							}, {
								"label": "电影",
								"value": 3
							}, {
								"label": "音乐",
								"value": 4
							}, {
								"label": "游戏",
								"value": 5
							}, {
								"label": "运动",
								"value": 6
							}]
						},
						"style": {},
						"size": "medium",
						"disabled": false,
						"__vModel__": "field112"
					}, {
						"__config__": {
							"label": "是否买房",
							"tag": "el-switch",
							"tagIcon": "switch",
							"defaultValue": false,
							"span": 24,
							"showLabel": true,
							"labelWidth": null,
							"layout": "colFormItem",
							"required": true,
							"regList": [],
							"changeTag": true,
							"document": "https://element.eleme.cn/#/zh-CN/component/switch",
							"formId": 113,
							"renderKey": 1624352180804
						},
						"style": {},
						"disabled": false,
						"active-text": "",
						"inactive-text": "",
						"active-color": null,
						"inactive-color": null,
						"active-value": true,
						"inactive-value": false,
						"__vModel__": "field113"
					}, {
						"__config__": {
							"label": "上传身份证",
							"tag": "el-upload",
							"tagIcon": "upload",
							"layout": "colFormItem",
							"defaultValue": null,
							"showLabel": true,
							"labelWidth": null,
							"required": true,
							"span": 24,
							"showTip": false,
							"buttonText": "点击上传",
							"regList": [],
							"changeTag": true,
							"fileSize": 2,
							"sizeUnit": "MB",
							"document": "https://element.eleme.cn/#/zh-CN/component/upload",
							"formId": 114,
							"renderKey": 1624352275010
						},
						"__slot__": {
							"list-type": true
						},
						"action": "https://jsonplaceholder.typicode.com/posts/",
						"disabled": false,
						"accept": "",
						"name": "file",
						"auto-upload": true,
						"list-type": "picture-card",
						"multiple": false,
						"__vModel__": "field114"
					}],
					"formRef": "elForm",
					"formModel": "formData",
					"size": "medium",
					"labelPosition": "right",
					"labelWidth": 100,
					"formRules": "rules",
					"gutter": 15,
					"disabled": false,
					"span": 24,
					"formBtns": true
				};
				this.parserisnot = false
				this.formParserData = jsonData.fields;
				this.$nextTick(function() {
					this.parserisnot = true
				});
			},


			// 表单提交
			onSubmit(data) {
				uni.showToast({
					title: '验证通过',
				});
				console.log('data:', data)
			},

		}
	}
</script>

<style>

</style>
