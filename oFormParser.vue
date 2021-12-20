<template>
	<view class="formParser-box">

		<!-- 动态表单 -->
		<u-form :model="form" ref="uForm" label-width="150">
			<block v-for="(item, index) in normalizedSize" :key="item.keyName">

				<u-form-item ref="uFormItem" :label="item.label" :prop="item.keyName" :required="item.required">

					<!-- input 输入框（单行文本，多行文本，密码框） -->
					<u-input v-if="item.tag == 'input'" v-bind:value="form[item.keyName]" :disabled="item.disabled"
						:type="item.type" :placeholder="item.placeholder" border
						@input="onChangeRate($event, index, item.keyName)" />

					<!-- 计数器 -->
					<u-number-box v-if="item.tag == 'input-number'" v-bind:value="form[item.keyName]" :min="item.min"
						:max="item.max" :step="item.step" :disabled="item.disabled"
						@change="onChangeNumber($event, index, item.keyName)">
					</u-number-box>

					<!-- 评分 -->
					<u-rate v-if="item.tag == 'rate'" :count="item.max" v-bind:value="form[item.keyName]"
						:disabled="item.disabled" size="32" gutter="15" inactive-color="#ccc" active-color="#F7BA2A"
						@change="onChangeRate($event, index, item.keyName)"></u-rate>

					<!-- 开关 -->
					<u-switch v-if="item.tag == 'switch'" v-bind:value="form[item.keyName]" :disabled="item.disabled"
						size="40" style="float:right;" @change="onChangeSwitch($event, item.keyName)"></u-switch>

					<!-- 单选框 -->
					<u-radio-group v-if="item.tag == 'radio'" :value="form[item.keyName]" :disabled="item.disabled">
						<u-radio v-for="(itemtwo, indextwo) in item.options" :key="indextwo" :name="itemtwo.value"
							@change="radioChange($event, item.keyName)">
							{{itemtwo.label}}
						</u-radio>
					</u-radio-group>

					<!-- 复选框 -->
					<u-checkbox-group v-if="item.tag == 'checkbox'" v-model="form[item.keyName]"
						:disabled="item.disabled">
						<u-checkbox v-model="itemtwo.checked" v-for="(itemtwo, indextwo) in item.options"
							:key="indextwo" :name="itemtwo.value"
							@change="checkboxChange($event, index, indextwo, item.keyName)">
							{{itemtwo.label}}
						</u-checkbox>
					</u-checkbox-group>

					<!-- 日期选择 -->
					<u-input v-if="item.tag == 'picker-date'" v-bind:value="form[item.keyName]"
						:disabled="item.disabled" :type="item.type" :placeholder="item.placeholder" border
						@click="onPickerDate(item.keyName)" />


					<!-- 时间选择 -->
					<u-input v-if="item.tag == 'picker-time'" v-bind:value="form[item.keyName]"
						:disabled="item.disabled" :type="item.type" :placeholder="item.placeholder" border
						@click="onPickerTime(item.keyName)" />


					<!-- 单列下拉 -->
					<!-- | filterValueToLabelSingle(item.options) -->
					<u-input v-if="item.tag == 'select-single'"
						v-bind:value="form[item.keyName] | filterValueToLabelMutil(item.options)"
						:disabled="item.disabled" :type="'select'" :select-open="showModelSelectSingle"
						:placeholder="item.placeholder" border
						@click="onSelectSingle(item.keyName, item.type, item.options)" />

					<!-- 级联下拉 -->
					<!-- | filterValueToLabelMutil(item.options) -->
					<u-input v-if="item.tag == 'select-mutil'"
						v-bind:value="form[item.keyName] | filterValueToLabelMutil(item.options)"
						:disabled="item.disabled" :type="'select'" :placeholder="item.placeholder" border
						@click="onSelectMutil(item.keyName, item.type, item.options)" />

					<!-- 图片上传 -->
					<u-upload v-if="item.tag == 'upload-image' && item.type == 'image'" ref="uploadFiles" width="160"
						height="160" :action="item.action" :file-list="item.fileList" :name="item.name"
						:auto-upload="item.autoUpload" :max-size="item.maxSize * 1024 * 1024" :max-count="item.maxCount"
						name="file" @on-uploaded="onUploadComplete($event, index, item.keyName)"></u-upload>

					<!-- 文件上传 -（未完成待定） -->
					<u-button v-if="item.tag == 'upload-image' && item.type == 'file'" type="primary" size="mini">
						<u-icon name="file-text" color="#fff" size="28"></u-icon>
						<text style="margin-left:5rpx;">上传文件</text>
					</u-button>


				</u-form-item>



			</block>
		</u-form>

		<!-- 日期弹窗 -->
		<u-picker mode="time" v-model="showModelDate" :params="paramsDate" @confirm="onConfirmDate"></u-picker>

		<!-- 时间弹窗 -->
		<u-picker mode="time" v-model="showModelTime" :params="paramsTime" @confirm="onConfirmTime"></u-picker>

		<!-- 单列下拉弹窗 -->
		<u-select v-model="showModelSelectSingle" :mode="selectType" :list="selectOptions"
			@confirm="onConfirmSelectSingle($event)"></u-select>

		<!-- 级联下拉弹窗 -->
		<u-select v-model="showModelSelectMutil" :mode="selectType" :list="selectOptions"
			@confirm="onConfirmSelectMutil($event)"></u-select>

		<!-- 表单按钮 -->
		<view class="form-footer">
			<u-row :gutter="20">
				<u-col :span="6">
					<u-button type="primary" ripple block @click="submit()">提交</u-button>
				</u-col>
				<u-col :span="6">
					<u-button ripple @click="reset()">取消</u-button>
				</u-col>
			</u-row>
		</view>


	</view>
</template>

<script>
	export default {
		name: "formParser",
		props: {
			// 父组件表单数据
			formParserData: {
				type: Array,
				default: () => []
			},
		},
		computed: {
			// 数据转换 可用数据normalizedSize
			normalizedSize() {

				console.log('computed')
				console.log(this.convertData(this.formParserData))
				return this.convertData(this.formParserData);
			}
		},

		data() {
			return {

				// 表单数据
				form: {},

				// 当前点击选择的keyName
				curKeyName: '',

				// 日期选择
				showModelDate: false,
				paramsDate: {
					year: true,
					month: true,
					day: true,
				},

				// 时间选择
				showModelTime: false,
				paramsTime: {
					hour: true,
					minute: true,
					second: true,
				},

				// 单列下拉
				showModelSelectSingle: false,

				// 级联下拉
				showModelSelectMutil: false,

				// 下拉组件参数
				selectType: '',
				selectOptions: [],

			};
		},

		filters: {
			// 单列下拉value转label
			filterValueToLabelSingle(value, arr) {
				console.log('value11:', value, 'arr:', arr)
				if (value) {
					console.log('ok')
					let label = '';
					arr.forEach(item => {
						if (item.value == value) {
							label = item.label;
						}
					})
					return label;
				}
			},

			// 级联下拉value转label
			filterValueToLabelMutil(value, arr) {
				if (value) {
					let val = value.split(',');
					let label = '';

					function keep(arr) {
						val.forEach((item, index) => {
							let arr2 = arr.filter(itemtwo => itemtwo.value == item);
							if (arr2.length) {
								index == 0 ? label += `${arr2[0].label}` : label += `/${arr2[0].label}`;
								if (arr2[0].children) {
									return keep(arr2[0].children);
								}
							}
						})
					};
					keep(arr);
					return label;
				}
			},
		},
		mounted() {
			console.log('mounted')
			this.setFormAndRules();
			this.$refs.uForm.setRules(this.rules);
		},
		methods: {
			// 数据转化为标准渲染格式
			convertData(formFields) {
				let objArray = [];

				formFields.forEach((item, index) => {
					const {
						disabled,
						maxlength,
						placeholder,
						__vModel__,
						__config__: {
							label,
							regList,
							required,
							tag,
							tagIcon,
							defaultValue
						}
					} = item;
					let obj = {
						keyName: __vModel__,
						value: String(defaultValue == undefined ? '' : defaultValue),
						label,
						tag: this.toTag(tag),
						type: this.toType(tagIcon),
						placeholder: placeholder == undefined ? this.toPlaceholder(label) : placeholder,
						required,
						regList,
						disabled,
					};

					let objData = {};
					// radio,checkbox,单选下拉，级联下拉，单独处理
					if (tag == 'el-radio-group' || tag == 'el-checkbox-group' || tag == 'el-select' || tag ==
						'el-cascader') {
						if (tag == 'el-cascader') {
							objData = {
								options: item.options
							}
						} else {
							objData = {
								options: item.__slot__.options
							}
						}
					}
					// rate单独处理
					if (tag == 'el-rate') {
						objData = {
							value: defaultValue == '0' ? '' : defaultValue,
							max: item.max,
							allowHalf: item['allow-half'],
						}
					}
					// 计数器number-box单独处理
					if (tag == 'el-input-number') {
						objData = {
							value: Number(defaultValue),
							min: item.min || '0',
							max: item.max || '999',
							step: item.step || '1',
						}
						console.log('okkkkk:', objData)
					}
					// 上传组件单upload单独处理
					if (tag == 'el-upload') {
						objData = {
							value: item.__config__.defaultValue,
							action: item.action,
							multiple: item.multiple,
							maxSize: item.__config__.fileSize,
							maxCount: 3,
							autoUpload: item['auto-upload'],
							name: item.name,
							fileList: item.fileList
						}
						if (item['list-type'] == 'picture-card') {
							obj.type = 'image';
						}
						if (item['list-type'] == 'text') {
							obj.type = 'file';
						}
					}

					Object.assign(obj, {
						...objData
					});
					objArray.push(obj);
				});
				console.log('objArray:', objArray);

				// console.log('formParserData1:', this.formParserData);
				return objArray;
			},

			// tag类型转换
			toTag(name) {
				let result = name;
				if (name == 'el-input') result = 'input';
				if (name == 'el-radio-group') result = 'radio';
				if (name == 'el-checkbox-group') result = 'checkbox';
				if (name == 'el-date-picker') result = 'picker-date';
				if (name == 'el-time-picker') result = 'picker-time';
				if (name == 'el-rate') result = 'rate';
				if (name == 'el-input-number') result = 'input-number';
				if (name == 'el-select') result = 'select-single';
				if (name == 'el-cascader') result = 'select-mutil';
				if (name == 'el-switch') result = 'switch';
				if (name == 'el-upload') result = 'upload-image';
				return result;
			},

			// type类型转换
			toType(name) {
				let result = name;
				if (name == 'input') result = 'text';
				if (name == 'number') result = 'input-number';
				if (name == 'date') result = 'select';
				if (name == 'time') result = 'select';
				if (name == 'select') result = 'single-column';
				if (name == 'cascader') result = 'mutil-column-auto';
				if (name == 'upload') result = 'image';
				return result;
			},

			// placeholder转换
			toPlaceholder(name) {
				return `请选择${name}`;
			},

			// 设置表单、验证数据
			setFormAndRules() {
				const {
					formParserData
				} = this;
				let tempRules = {};
				let tempForm = {};
				this.normalizedSize.forEach((item, index) => {
					// 表单属性添加
					tempForm[item.keyName] = item.value || null;

					let arr = [];
					// 必填验证添加
					if (item.required && item.tag != 'switch') {
						const obj = {
							required: true,
							message: item.placeholder,
							trigger: ['change', 'blur'],
						}
						arr.push(obj);
					}
					// 正则验证添加
					if (item.regList.length) {
						item.regList.forEach((itemtwo, indextwo) => {
							const obj = {
								validator: (rule, value, callback) => {
									return new RegExp(itemtwo.pattern).test(value)
								},
								message: itemtwo.message,
								trigger: ['change', 'blur'],
							}
							arr.push(obj);
						})
					}
					Object.assign(tempRules, {
						[`${item.keyName}`]: arr
					});
				})
				this.form = tempForm;
				this.rules = tempRules;
				console.log('form:', this.form)
				console.log('rules:', this.rules)
			},

			// 表单提交
			submit() {
				console.log('form', this.form)
				this.$refs.uForm.validate(valid => {
					if (valid) {
						this.$emit('eventSubmit', this.form);
					} else {
						console.log('验证失败');
					}
				});
			},

			// 表单重置
			reset() {
				// 重置checkbox数据
				let tempFormParserData = this.formParserData;
				Object.keys(this.form).forEach(key => this.form[key] = '');
				tempFormParserData.forEach(item => {
					if (item.tag == 'checkbox') {
						item.options.forEach(itemtwo => itemtwo.checked = false);
					}
				})
				this.$parent.formParserData = tempFormParserData;
				console.log('formParserData:', this.formParserData)
				// 重置上传upload数据
				this.$refs.uploadFiles.forEach(item => item.clear());
				// 重置表单
				setTimeout(() => {
					this.$refs.uForm.resetFields();
					console.log('form12:', this.form)
				}, 300)
			},

			// 评分 change事件
			onChangeRate(e, index, keyName) {
				console.log('e:', e, 'index:', index)
				this.form[keyName] = e;
				setTimeout(() => {
					this.$refs.uFormItem[index].validation(); // 验证单条form-item规则
				}, 1000)
			},

			// 进步器change事件
			onChangeNumber(e, index, keyName) {
				console.log('e:', e, 'index:', index)
				this.form[keyName] = e.value;
				setTimeout(() => {
					this.$refs.uFormItem[index].validation(); // 验证单条form-item规则
				}, 300)
			},

			// 开关change事件
			onChangeSwitch(e, keyName) {
				console.log('e:', e, 'keyname:', keyName)
				this.form[keyName] = !e;
			},

			// 单选框 change事件
			radioChange(e, keyName) {
				console.log('e:', e)
				this.form[keyName] = String(e);
			},

			// 复选框 change事件
			checkboxChange(e, indexone, indextwo, keyName) {
				const {
					value,
					name
				} = e;
				console.log('e2:', e, 'indexone:', indexone, 'indextwo:', indextwo)
				this.$parent.formParserData[indexone].options[indextwo].checked = value;
				let arr1 = this.$parent.formParserData[indexone].options.filter(item => item.checked).map(item => item
					.value);
				this.form[keyName] = arr1.toString();
			},

			// 日期弹窗显隐
			onPickerDate(keyName) {
				this.showModelDate = true;
				this.curKeyName = keyName;
			},

			// 日期确认选择
			onConfirmDate(e) {
				const {
					year,
					month,
					day
				} = e;
				const {
					curKeyName
				} = this;
				this.form[curKeyName] = `${year}-${month}-${day}`;
				console.log('e:', e, 'curKeyName:', curKeyName)
			},

			// 时间弹窗显隐
			onPickerTime(keyName) {
				this.showModelTime = true;
				this.curKeyName = keyName;
			},

			// 时间确认选择
			onConfirmTime(e) {
				const {
					hour,
					minute,
					second
				} = e;
				const {
					curKeyName
				} = this;
				this.form[curKeyName] = `${hour}:${minute}:${second}`;
				console.log('e:', e, 'curKeyName:', curKeyName)
			},

			// 单列下拉弹窗显隐
			onSelectSingle(keyName, type, options) {
				this.showModelSelectSingle = true;
				this.curKeyName = keyName;
				this.selectOptions = options;
				this.selectType = type;
				console.log('selectOptions:', this.selectOptions)
			},

			// 单列下拉确认选择
			onConfirmSelectSingle(e, keyName) {
				console.log('e:', e, 'key:', keyName)
				const {
					label,
					value
				} = e[0];
				const {
					curKeyName
				} = this;
				this.form[curKeyName] = String(value);
			},

			// 级联下拉弹窗显隐
			onSelectMutil(keyName, type, options) {
				this.showModelSelectMutil = true;
				this.curKeyName = keyName;
				this.selectOptions = options;
				this.selectType = type;
				console.log('selectOptions:', this.selectOptions)
			},

			// 级联下拉确认选择
			onConfirmSelectMutil(e, keyName) {
				console.log('e:', e, 'key:', keyName)
				let arr = e.map(item => item.value);
				const {
					curKeyName
				} = this;
				this.form[curKeyName] = arr.join(',');
			},

			// 图片上传成功回调
			onUploadComplete(lists, index, keyName) {
				console.log('onUploaded', lists, 'keyName:', keyName);
				let arr = [];
				lists.forEach((item, index) => {
					console.log('item', item)
					if (item.progress == 100) {
						if (item.hasOwnProperty("response")) {
							let json = {
								name: new Date( +new Date() + 8 * 3600 * 1000 ).toJSON().substr(0,10).replace("T"," ")+'日图片' + index + 1,
								url: item.response.url,
							}
							arr.push(json);
						} else {
							let json = {
								name: new Date( +new Date() + 8 * 3600 * 1000 ).toJSON().substr(0,10).replace("T"," ")+'日图片' + index + 1,
								url: item.url,
							}
							arr.push(json);
						}
					}
				})
				this.form[keyName] = arr;
				setTimeout(() => {
					this.$refs.uFormItem[index].validation(); // 验证单条form-item规则
				}, 100)
			},

			// 图片上传chnage事件
			// onUploadListChange(lists, index, keyName) {
			// 	console.log('lists', lists, 'keyName:', keyName);
			// 	let arr = [];
			// 	lists.forEach(item => {
			// 		if (item.progress == 100) {
			// 			if (item.hasOwnProperty("response")) {
			// 				let json = {
			// 					name: item.response.url,
			// 					url: item.response.url,
			// 				}
			// 				arr.push(json);
			// 			} else {
			// 				let json = {
			// 					name: item.url,
			// 					url: item.url,
			// 				}
			// 				arr.push(json);
			// 			}
			// 		}
			// 	})
			// 	this.form[keyName] = arr;
			// 	console.log(this.form[keyName])
			// 	setTimeout(() => {
			// 		this.$refs.uFormItem[index].validation(); // 验证单条form-item规则
			// 	}, 100)
			// },



		}
	}
</script>

<style lang="scss">
	.formParser-box {
		padding: 25rpx;

		.form-footer {
			padding: 60rpx 0;
		}
	}
</style>
