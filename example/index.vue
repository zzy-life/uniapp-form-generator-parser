<template>
	<view>

		<oFormParser v-if="parserisnot" :formParserData="formParserData" @eventSubmit="onSubmit"></oFormParser>
		<!-- <u-select v-model="show" mode="single-column" :list="list" @confirm="confirm"></u-select> -->
	</view>
</template>

<script>
	import oFormParser from 'uniapp-form-generator-parser/oFormParser.vue';

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
							"label": "账号",
							"labelWidth": null,
							"showLabel": true,
							"changeTag": true,
							"tag": "el-input",
							"tagIcon": "input",
							"defaultValue": "79000",
							"required": true,
							"layout": "colFormItem",
							"span": 24,
							"document": "https://element.eleme.cn/#/zh-CN/component/input",
							"regList": [],
							"formId": 101,
							"renderKey": "1011639670663972"
						},
						"__slot__": {
							"prepend": "",
							"append": ""
						},
						"placeholder": "请输入账号",
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
							"label": "密码",
							"showLabel": true,
							"labelWidth": null,
							"changeTag": true,
							"tag": "el-input",
							"tagIcon": "password",
							"defaultValue": "123456",
							"layout": "colFormItem",
							"span": 24,
							"required": true,
							"regList": [],
							"document": "https://element.eleme.cn/#/zh-CN/component/input",
							"formId": 102,
							"renderKey": "1021639670664843"
						},
						"__slot__": {
							"prepend": "",
							"append": ""
						},
						"placeholder": "请输入密码",
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
						"__vModel__": "field102"
					}, {
						"__config__": {
							"label": "性别",
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
							"formId": 103,
							"renderKey": "1031639670684974",
							"defaultValue": 1
						},
						"__slot__": {
							"options": [{
								"label": "男",
								"value": 1
							}, {
								"label": "女",
								"value": 2
							}]
						},
						"placeholder": "请选择性别",
						"style": {
							"width": "100%"
						},
						"clearable": true,
						"disabled": false,
						"filterable": false,
						"multiple": false,
						"__vModel__": "field103"
					}, {
						"__config__": {
							"label": "图片上传",
							"tag": "el-upload",
							"tagIcon": "upload",
							"layout": "colFormItem",
							"defaultValue": [{
								"name": "erciyuan.jpg",
								"url": "http://www.zhangzhiyu.live:8088/xyw/profile/upload/2021/12/17/6fb64b87-547c-48ed-968e-dd0165f609a4.jpg"
							}],
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
							"formId": 104,
							"renderKey": "1041639670702247"
						},
						"__slot__": {
							"list-type": true
						},
						"action": "http://www.zhangzhiyu.live:8900/xyw/prod-api/common/upload",
						"disabled": false,
						"accept": "",
						"name": "file",
						"auto-upload": true,
						"list-type": "picture",
						"multiple": false,
						"__vModel__": "field104"
					}, {
						"__config__": {
							"label": "附件上传",
							"tag": "el-upload",
							"tagIcon": "upload",
							"layout": "colFormItem",
							"defaultValue": [{
								"name": "环保题库.xlsx",
								"url": "http://www.zhangzhiyu.live:8088/xyw/profile/upload/2021/12/17/e48bc10b-a93d-4cb6-868b-f86c03e4297c.xlsx"
							}],
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
							"formId": 105,
							"renderKey": "1051639670714315"
						},
						"__slot__": {
							"list-type": true
						},
						"action": "/xyw/prod-api/common/upload",
						"disabled": false,
						"accept": "",
						"name": "file",
						"auto-upload": true,
						"list-type": "text",
						"multiple": false,
						"__vModel__": "field105"
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
				this.backfillFormData(jsonData);//回填图片
				this.formParserData = jsonData.fields;
				console.log(this.formParserData)
				this.$nextTick(function() {
					this.parserisnot = true
				});
			},
			//回填图片
			backfillFormData(form) {
				form.fields.forEach((item) => {
					if (item.__config__.tag == "el-upload") {
						//如果是图片
						const val = item.__config__.defaultValue; //拿出值
						if (val == null) {
							//如果等于null直接跳出
							return;
						}
						if (item.hasOwnProperty("fileList")) {
							//如果没有这个key
							item["fileList"].splice(0, item["fileList"].length); //清空回显
						}
						for (let index = 0; index < val.length; index++) {
							//循环值
							if (!item.hasOwnProperty("fileList")) {
								//如果没有这个key
								item["fileList"] = [
									//直接定义一个数组并赋值
									{
										name: val[index].name,
										url: val[index].url,
									},
								];
							} else {
								item["fileList"].push({
									//追加进去
									name: val[index].name,
									url: val[index].url,
								});
							}
						}
					}
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
