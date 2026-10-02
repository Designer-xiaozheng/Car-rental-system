<template>
	<el-main class="bg">
		<el-form ref="form" :model="form" status-icon label-width="120px" v-if="is_view()">
			<el-col v-if="user_group === '管理员' || $check_field('get','item_name') || $check_field('add','item_name') || $check_field('set','item_name')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="物品名称" prop="item_name">
					<el-input id="item_name" v-model="form['item_name']" placeholder="请输入物品名称"
							  v-if="user_group === '管理员' || (form['second_hand_transaction_id'] && $check_field('set','item_name')) || (!form['second_hand_transaction_id'] && $check_field('add','item_name'))" :disabled="disabledObj['item_name_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','item_name')">{{form['item_name']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','item_type') || $check_field('add','item_type') || $check_field('set','item_type')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="物品类型" prop="item_type">
					<el-input id="item_type" v-model="form['item_type']" placeholder="请输入物品类型"
							  v-if="user_group === '管理员' || (form['second_hand_transaction_id'] && $check_field('set','item_type')) || (!form['second_hand_transaction_id'] && $check_field('add','item_type'))" :disabled="disabledObj['item_type_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','item_type')">{{form['item_type']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','article_fineness') || $check_field('add','article_fineness') || $check_field('set','article_fineness')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="物品成色" prop="article_fineness">
					<el-input id="article_fineness" v-model="form['article_fineness']" placeholder="请输入物品成色"
							  v-if="user_group === '管理员' || (form['second_hand_transaction_id'] && $check_field('set','article_fineness')) || (!form['second_hand_transaction_id'] && $check_field('add','article_fineness'))" :disabled="disabledObj['article_fineness_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','article_fineness')">{{form['article_fineness']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','item_picture') || $check_field('add','item_picture') || $check_field('set','item_picture')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="物品图片" prop="item_picture">
					<el-upload :disabled="disabledObj['item_picture_isDisabled']" id="item_picture" class="avatar-uploader" drag
						accept="image/gif, image/jpeg, image/png, image/jpg" action="" :http-request="upload_item_picture"
						:show-file-list="false" v-if="user_group === '管理员' || (form['second_hand_transaction_id'] && $check_field('set','item_picture')) || (!form['second_hand_transaction_id'] && $check_field('add','item_picture'))">
						<img v-if="form['item_picture']" :src="$fullUrl(form['item_picture'])" class="avatar">
						<i v-else class="el-icon-plus avatar-uploader-icon"></i>
					</el-upload>
					<el-image v-else-if="$check_field('get','item_picture')" style="width: 100px; height: 100px"
						:src="$fullUrl(form['item_picture'])" :preview-src-list="[$fullUrl(form['item_picture'])]">
						<div slot="error" class="image-slot">
							<img src="../../../public/img/error.png" style="width: 90px; height: 90px" />
						</div>
					</el-image>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','item_price') || $check_field('add','item_price') || $check_field('set','item_price')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="物品价格" prop="item_price">
					<el-input id="item_price" v-model="form['item_price']" placeholder="请输入物品价格"
							  v-if="user_group === '管理员' || (form['second_hand_transaction_id'] && $check_field('set','item_price')) || (!form['second_hand_transaction_id'] && $check_field('add','item_price'))" :disabled="disabledObj['item_price_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','item_price')">{{form['item_price']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','user_number') || $check_field('add','user_number') || $check_field('set','user_number')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="用户编号" prop="user_number">
						<div v-if="user_group !== '管理员'">
							{{ get_user_session_user_number(form['user_number']) }}
							<!--<el-input id="business_name" v-model="form['user_number']" placeholder="请输入用户编号"-->
							<!--v-if="user_group === '管理员' || (form['second_hand_transaction_id'] && $check_field('set','user_number')) || (!form['second_hand_transaction_id'] && $check_field('add','user_number'))" :disabled="disabledObj['user_number_isDisabled']"></el-input>-->
							<!--<div v-else-if="$check_field('get','user_number')">{{form['user_number']}}</div>-->
							<el-select v-if="user_group === '管理员' || (form['second_hand_transaction_id'] && $check_field('set','user_number')) || (!form['second_hand_transaction_id'] && $check_field('add','user_number'))" id="user_number" v-model="form['user_number']" :disabled="disabledObj['user_number_isDisabled']">
								<el-option v-for="o in list_user_user_number" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
										   :value="o['user_id']">
								</el-option>
							</el-select>
							<el-select v-else-if="$check_field('get','user_number')" id="user_number" v-model="form['user_number']" :disabled="true">
								<el-option v-for="o in list_user_user_number" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
										   :value="o['user_id']">
								</el-option>
							</el-select>
						</div>
						<el-select v-else id="user_number" v-model="form['user_number']" :disabled="disabledObj['user_number_isDisabled']">
							<el-option v-for="o in list_user_user_number" :key="o['username']" :label="o['nickname'] + '-' + o['username']"
									   :value="o['user_id']">
							</el-option>
						</el-select>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','user_mobile_number') || $check_field('add','user_mobile_number') || $check_field('set','user_mobile_number')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="用户手机号" prop="user_mobile_number">
					<el-input id="user_mobile_number" v-model="form['user_mobile_number']" placeholder="请输入用户手机号" type="tel"
						v-if="user_group === '管理员' || (form['second_hand_transaction_id'] && $check_field('set','user_mobile_number')) || (!form['second_hand_transaction_id'] && $check_field('add','user_mobile_number'))">
					</el-input>
					<div v-else-if="$check_field('get','user_mobile_number')">{{form['user_mobile_number']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','transaction_status') || $check_field('add','transaction_status') || $check_field('set','transaction_status')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="交易状态" prop="transaction_status">
					<el-select id="transaction_status" v-model="form['transaction_status']"
						v-if="user_group === '管理员' || (form['second_hand_transaction_id'] && $check_field('set','transaction_status')) || (!form['second_hand_transaction_id'] && $check_field('add','transaction_status'))">
						<el-option v-for="o in list_transaction_status" :key="o" :label="o" :value="o">
						</el-option>
					</el-select>
					<div v-else-if="$check_field('get','transaction_status')">{{form['transaction_status']}}</div>
				</el-form-item>
			</el-col>
			<el-col v-if="user_group === '管理员' || $check_field('get','item_details') || $check_field('add','item_details') || $check_field('set','item_details')" :xs="24" :sm="12" :lg="8">
				<el-form-item label="物品详情" prop="item_details">
					<el-input type="textarea" id="item_details" v-model="form['item_details']" placeholder="请输入物品详情"
						v-if="user_group === '管理员' || (form['second_hand_transaction_id'] && $check_field('set','item_details')) || (!form['second_hand_transaction_id'] && $check_field('add','item_details'))" :disabled="disabledObj['item_details_isDisabled']"></el-input>
					<div v-else-if="$check_field('get','item_details')">{{form['item_details']}}</div>
				</el-form-item>
			</el-col>
			<el-col :xs="24" :sm="12" :lg="8">
				<el-form-item>
					<el-button type="primary" @click="submit()">提交</el-button>
					<el-button @click="cancel()">取消</el-button>
				</el-form-item>
			</el-col>

		</el-form>
	</el-main>
</template>

<script>
	import mixin from "@/mixins/page.js";

	export default {
		mixins: [mixin],
		data() {
			return {
				field: "second_hand_transaction_id",
				url_add: "~/api/second_hand_transaction/add?",
				url_set: "~/api/second_hand_transaction/set?",
				url_get_obj: "~/api/second_hand_transaction/get_obj?",
				url_upload: "~/api/second_hand_transaction/upload?",

				query: {
					"second_hand_transaction_id": 0,
				},

				form: {
					"item_name":'', // 物品名称
					"item_type":'', // 物品类型
					"article_fineness":'', // 物品成色
					"item_picture":'', // 物品图片
					"item_price":'', // 物品价格
					"user_number": 0, // 用户编号
					"user_mobile_number":'', // 用户手机号
					"transaction_status":'', // 交易状态
					"item_details":'', // 物品详情
					"second_hand_transaction_id": 0, // ID

				},
				disabledObj:{
					"item_name_isDisabled": false,
					"item_type_isDisabled": false,
					"article_fineness_isDisabled": false,
					"item_picture_isDisabled": false,
					"item_price_isDisabled": false,
					"user_number_isDisabled": false,
					"user_mobile_number_isDisabled": false,
					"transaction_status_isDisabled": false,
					"item_details_isDisabled": false,
				},
				// 用户列表
				list_user_user_number: [],
				// 用户组
				group_user_user_number: "",
				//交易状态选项列表
				list_transaction_status: ['上架中','已下架'],

			}
		},
		methods: {
			/**
			 * 上传物品图片
			 * @param {Object} param图片参数
			 */
			upload_item_picture(param){
				this.uploadFile(param.file, "item_picture");
			},
			/**
			 * 获取普通用户用户列表
			 */
			async get_list_user_user_number() {
                // if(this.user_group !== "管理员" && this.form["user_number"] === 0) {
                //     this.form["user_number"] = this.user.user_id;
                // }
                var json = await this.$get("~/api/user/get_list?user_group=普通用户");
                if(json.result && json.result.list){
                    this.list_user_user_number = json.result.list;
                }
                else if(json.error){
                    console.error(json.error);
                }
			},
			/**
			 * 获取普通用户用户组
			 */
			async get_group_user_user_number() {
				this.form["user_number"] = this.user.user_id;
				var json = await this.$get("~/api/user_group/get_obj?name=普通用户");
				if(json.result && json.result.obj){
					this.group_user_user_number = json.result.obj;
				}
				else if(json.error){
					console.error(json.error);
				}
			},
			get_user_session_user_number(id){
				var _this = this;
				var user_id = {"user_id":id}
				var url = "~/api/"+_this.group_user_user_number.source_table+"/get_obj?"
				this.$get(url, user_id, function(res) {
					if (res.result && res.result.obj) {
						var arr = []
						for (let key in res.result.obj) {
							arr.push(key)
						}
						var arrForm = []
						for (let key in _this.form) {
							arrForm.push(key)
						}
						for (var i=0;i<arr.length;i++){
							for (var j=0;j<arrForm.length;j++){
								if (arr[i]===arrForm[j]){
									if (arr[i]!=="user_number") {
										_this.form[arrForm[j]] = res.result.obj[arr[i]]
										_this.disabledObj[arrForm[j] + '_isDisabled'] = true
										break;
									}else {
										_this.disabledObj[arrForm[j] + '_isDisabled'] = true
									}
								}
							}
						}
					}
				});
			},
			get_user_user_number(id){
				var obj = this.list_user_user_number.getObj({"user_id":id});
				var ret = "";
				if(obj){
					if(obj.nickname){
						ret = obj.nickname;}
					else{
						ret = obj.username;
					}
				}
				return ret;
			},

			/**
			 * 获取对象之前
			 * @param {Object} param
			 */
			get_obj_before(param) {
				var form = "";
				/**
				* 请求列表前
				* @param {Object} param
				*/
				var user_group = this.user.user_group;
				if (user_group !== "管理员") {
					switch (user_group) {
						case "用户编号":
							if(param["user_number"] > 0){
								param["user_number"] = this.user.user_id;
							}
							break;
					}
				}
				if(this.form && form){
					Object.keys(this.form).forEach(key => {
						Object.keys(form).forEach(dbKey => {
							if(dbKey === "charging_standard"){
								this.form['charging_rules'] = form[dbKey];
								this.disabledObj['charging_rules_isDisabled'] = true;
							};
							if(key === dbKey){
								this.disabledObj[key+'_isDisabled'] = true;
							}
						})
					})
				}
				$.db.del("form");
				return param;
			},

			/**
			 * 获取对象之后
			 * @param {Object} json
			 * @param {Object} func
			 */
			get_obj_after(json, func){

			},

			is_view(){
				var bl = this.user_group == "管理员";

				if(!bl){
					bl = this.$check_action('/second_hand_transaction/table','add');
					console.log(bl ? "你有表格添加权限视作有添加权限" : "你没有表格添加权限");
				}
				if(!bl){
					bl = this.$check_action('/second_hand_transaction/table','set');
					console.log(bl ? "你有表格添加权限视作有修改权限" : "你没有表格修改权限");
				}
				if(!bl){
					bl = this.$check_action('/second_hand_transaction/view','add');
					console.log(bl ? "你有视图添加权限视作有添加权限" : "你没有视图添加权限");
				}
				if(!bl){
					bl = this.$check_action('/second_hand_transaction/view','set');
					console.log(bl ? "你有视图修改权限视作有修改权限" : "你没有视图修改权限");
				}
				if(!bl){
					bl = this.$check_action('/second_hand_transaction/view','get');
					console.log(bl ? "你有视图查询权限视作有查询权限" : "你没有视图查询权限");
				}

				console.log(bl ? "具有当前页面的查看权，请注意这不代表你有字段的查看权" : "无权查看当前页，请注意即便有字段查询权限没有页面查询权限也不行");

				return bl;
			},
			/**
			 * 上传文件
			 * @param {Object} param
			 */
			uploadimg(param) {
				this.uploadFile(param.file, "avatar");
			},

		},
		created() {
			this.get_list_user_user_number();
			this.get_group_user_user_number();
		}
	}
</script>

<style>
	.avatar-uploader .el-upload {
		border: 1px dashed #d9d9d9;
		border-radius: 6px;
		cursor: pointer;
		position: relative;
		overflow: hidden;
	}

	.avatar-uploader .el-upload:hover {
		border-color: #409EFF;
	}

	.avatar-uploader-icon {
		font-size: 28px;
		color: #8c939d;
		width: 178px;
		height: 178px;
		line-height: 178px;
		text-align: center;
	}

	.avatar {
		width: 178px;
		height: 178px;
		display: block;
	}
</style>
