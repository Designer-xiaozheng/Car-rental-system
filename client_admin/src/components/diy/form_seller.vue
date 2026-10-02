<template>
	<div>
		<el-form-item v-if="$check_register_field('add','seller_name','/seller/view')" label="卖家姓名" prop="seller_name">
		<el-input id="seller_name" v-model="form['seller_name']" placeholder="请输入卖家姓名"></el-input>
		</el-form-item>
		<el-form-item v-if="$check_register_field('add','id_number','/seller/view')" label="身份证号" prop="id_number">
		<el-input id="id_number" v-model="form['id_number']" placeholder="请输入身份证号"></el-input>
		</el-form-item>
		<el-form-item v-if="$check_register_field('add','store_name','/seller/view')" label="店铺名称" prop="store_name">
		<el-input id="store_name" v-model="form['store_name']" placeholder="请输入店铺名称"></el-input>
		</el-form-item>
		<el-form-item v-if="$check_register_field('add','store_location','/seller/view')" label="店铺位置" prop="store_location">
		<el-input id="store_location" v-model="form['store_location']" placeholder="请输入店铺位置"></el-input>
		</el-form-item>
		<el-form-item v-if="$check_register_field('add','nature_of_business','/seller/view')" label="经营范围" prop="nature_of_business">
		<el-input id="nature_of_business" v-model="form['nature_of_business']" placeholder="请输入经营范围"></el-input>
		</el-form-item>
		<el-form-item v-if="$check_register_field('add','shop_features','/seller/view')" label="店铺特色" prop="shop_features">
		<el-input id="shop_features" v-model="form['shop_features']" placeholder="请输入店铺特色"></el-input>
		</el-form-item>
		<el-form-item v-if="$check_register_field('add','shop_number','/seller/view')" label="店铺编号" prop="shop_number">
		<el-input id="shop_number" v-model="form['shop_number']" placeholder="请输入店铺编号"></el-input>
		</el-form-item>
		<el-form-item v-if="$check_register_field('add','relevant_certificates','/seller/view')" label="相关证件" prop="relevant_certificates">
		<el-upload id="relevant_certificates" class="avatar-uploader" drag
			accept="image/gif, image/jpeg, image/png, image/jpg" action="" :http-request="uploadimg"
			:show-file-list="false" >
			<img v-if="form['relevant_certificates']" :src="$fullUrl(form['relevant_certificates'])" class="avatar">
			<i v-else class="el-icon-plus avatar-uploader-icon"></i>
		</el-upload>
		</el-form-item>

	</div>
</template>

<script>
	import mixin from "@/mixins/component.js";

	export default {
		mixins: [mixin],
		props:{

			form:{
				type: Object,
				default: function(){
					return {
						"seller_name":'',
						"id_number":'',
						"store_name":'',
						"store_location":'',
						"nature_of_business":'',
						"shop_features":'',
						"shop_number":'',
						"relevant_certificates":'',
					}
				}
			},

		},
		data() {
			return {
				field: "seller_id",
				url_add: "~/api/seller/add?",
				url_set: "~/api/seller/set?",
				url_upload: "~/api/seller/upload?",

				rules: {
					"seller_name": [ ],
					"id_number": [ ],
					"store_name": [ ],
					"store_location": [ ],
					"nature_of_business": [ ],
					"shop_features": [ ],
					"shop_number": [ {required: true,message: '店铺编号不能为空'}, ],
					"relevant_certificates": [ ],
				}

			}
		},
		methods: {
			/**
			 * 上传相关证件
			 * @param {Object} param 图片参数
			 */
			uploadimg(param) {
				this.uploadFile(param.file, "relevant_certificates");
			},

		},
		created() {
		}
	}
</script>
