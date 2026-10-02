<template>
  <div class="page_search">
	<div class="warp">
	  <div class="container">
		<div class="row">
		  <div class="col-12">
			<div class="card_result_search">
			  <div class="title">搜索结果</div>
				<!-- 商品搜索结果 -->
			  <list_result_search
				:list="result_goods"
				title="购物商城"
				source_table="goods"
			  ></list_result_search>
				<!-- 文章搜索结果 -->
			  <list_result_search
				:list="result_article"
				title="新闻资讯"
				source_table="article"
			  ></list_result_search>
				<!-- 论坛搜索结果 -->
			  <list_result_search
				:list="result_forum"
				title="交流论坛"
				source_table="forum"
			  ></list_result_search>
			  <list_result_search
				v-if="$check_action('/seller/list', 'get')"
				:list="result_seller_seller_name"
				title="卖家卖家姓名"
				source_table="seller"
			  ></list_result_search>
			  <list_result_search
				v-if="$check_action('/seller/list', 'get')"
				:list="result_seller_store_name"
				title="卖家店铺名称"
				source_table="seller"
			  ></list_result_search>
			  <list_result_search
				v-if="$check_action('/seller/list', 'get')"
				:list="result_seller_store_location"
				title="卖家店铺位置"
				source_table="seller"
			  ></list_result_search>
			  <list_result_search
				v-if="$check_action('/ordinary_users/list', 'get')"
				:list="result_ordinary_users_user_name"
				title="普通用户用户姓名"
				source_table="ordinary_users"
			  ></list_result_search>
			  <list_result_search
				v-if="$check_action('/ordinary_users/list', 'get')"
				:list="result_ordinary_users_user_gender"
				title="普通用户用户性别"
				source_table="ordinary_users"
			  ></list_result_search>
			  <list_result_search
				v-if="$check_action('/ordinary_users/list', 'get')"
				:list="result_ordinary_users_user_number"
				title="普通用户用户编号"
				source_table="ordinary_users"
			  ></list_result_search>
			  <list_result_search
				v-if="$check_action('/shopping_mall/list', 'get')"
				:list="result_shopping_mall_store_name"
				title="商城店铺名称"
				source_table="shopping_mall"
			  ></list_result_search>
			  <list_result_search
				v-if="$check_action('/second_hand_transaction/list', 'get')"
				:list="result_second_hand_transaction_item_name"
				title="二手交易物品名称"
				source_table="second_hand_transaction"
			  ></list_result_search>
			  <list_result_search
				v-if="$check_action('/second_hand_transaction/list', 'get')"
				:list="result_second_hand_transaction_item_type"
				title="二手交易物品类型"
				source_table="second_hand_transaction"
			  ></list_result_search>
			  <list_result_search
				v-if="$check_action('/second_hand_transaction/list', 'get')"
				:list="result_second_hand_transaction_transaction_status"
				title="二手交易交易状态"
				source_table="second_hand_transaction"
			  ></list_result_search>
			</div>
		  </div>
		</div>
	  </div>
	</div>
  </div>
</template>

<script>
import mixin from "../../mixins/page.js";
import list_result_search from "../../components/diy/list_result_search.vue";

export default {
  mixins: [mixin],
  data() {
	return {
	  "query": {
		word: "",
	  },
	  "result_goods": [],
	  "result_article": [],
	  "result_forum": [],
			"result_seller_seller_name":[],
			"result_seller_store_name":[],
			"result_seller_store_location":[],
			"result_ordinary_users_user_name":[],
			"result_ordinary_users_user_gender":[],
			"result_ordinary_users_user_number":[],
			"result_shopping_mall_store_name":[],
			"result_second_hand_transaction_item_name":[],
			"result_second_hand_transaction_item_type":[],
			"result_second_hand_transaction_transaction_status":[],
	};
  },
  methods: {
	/**
	 * 获取购物商城
	 */
	get_goods() {
	  this.$get("~/api/goods/get_list?like=0", { page: 1, size: 10, title: this.query.word }, (json) => {
		if (json.result) {
		  this.result_goods = json.result.list;
		}
	  });
	},
	/**
	 * 获取文章
	 */
	get_article() {
	  this.$get("~/api/article/get_list?like=0", { page: 1, size: 10, title: this.query.word }, (json) => {
		if (json.result) {
		  this.result_article = json.result.list;
		}
	  });
	},
	/**
	 * 获取交流论坛
	 */
	get_forum() {
	  this.$get("~/api/forum/get_list?like=0", { page: 1, size: 10, title: this.query.word }, (json) => {
		if (json.result) {
		  this.result_forum = json.result.list;
		}
	  });
	},
	/**
	 * 获取seller_name
	 */
	get_seller_seller_name(){
		this.$get("~/api/seller/get_list?like=0", { page: 1, size: 10, "seller_name": this.query.word }, (json) => {
		  if (json.result) {
			var result_seller_seller_name = json.result.list;
			result_seller_seller_name.map(o => o.title = o['seller_name'])
	  			this.result_seller_seller_name = result_seller_seller_name
		 	}
		});
	},
	/**
	 * 获取store_name
	 */
	get_seller_store_name(){
		this.$get("~/api/seller/get_list?like=0", { page: 1, size: 10, "store_name": this.query.word }, (json) => {
		  if (json.result) {
			var result_seller_store_name = json.result.list;
			result_seller_store_name.map(o => o.title = o['store_name'])
	  			this.result_seller_store_name = result_seller_store_name
		 	}
		});
	},
	/**
	 * 获取store_location
	 */
	get_seller_store_location(){
		this.$get("~/api/seller/get_list?like=0", { page: 1, size: 10, "store_location": this.query.word }, (json) => {
		  if (json.result) {
			var result_seller_store_location = json.result.list;
			result_seller_store_location.map(o => o.title = o['store_location'])
	  			this.result_seller_store_location = result_seller_store_location
		 	}
		});
	},
	/**
	 * 获取user_name
	 */
	get_ordinary_users_user_name(){
		this.$get("~/api/ordinary_users/get_list?like=0", { page: 1, size: 10, "user_name": this.query.word }, (json) => {
		  if (json.result) {
			var result_ordinary_users_user_name = json.result.list;
			result_ordinary_users_user_name.map(o => o.title = o['user_name'])
	  			this.result_ordinary_users_user_name = result_ordinary_users_user_name
		 	}
		});
	},
	/**
	 * 获取user_gender
	 */
	get_ordinary_users_user_gender(){
		this.$get("~/api/ordinary_users/get_list?like=0", { page: 1, size: 10, "user_gender": this.query.word }, (json) => {
		  if (json.result) {
			var result_ordinary_users_user_gender = json.result.list;
			result_ordinary_users_user_gender.map(o => o.title = o['user_gender'])
	  			this.result_ordinary_users_user_gender = result_ordinary_users_user_gender
		 	}
		});
	},
	/**
	 * 获取user_number
	 */
	get_ordinary_users_user_number(){
		this.$get("~/api/ordinary_users/get_list?like=0", { page: 1, size: 10, "user_number": this.query.word }, (json) => {
		  if (json.result) {
			var result_ordinary_users_user_number = json.result.list;
			result_ordinary_users_user_number.map(o => o.title = o['user_number'])
	  			this.result_ordinary_users_user_number = result_ordinary_users_user_number
		 	}
		});
	},
	/**
	 * 获取store_name
	 */
	get_shopping_mall_store_name(){
		this.$get("~/api/shopping_mall/get_list?like=0", { page: 1, size: 10, "store_name": this.query.word }, (json) => {
		  if (json.result) {
			var result_shopping_mall_store_name = json.result.list;
			result_shopping_mall_store_name.map(o => o.title = o['store_name'])
	  			this.result_shopping_mall_store_name = result_shopping_mall_store_name
		 	}
		});
	},
	/**
	 * 获取item_name
	 */
	get_second_hand_transaction_item_name(){
		this.$get("~/api/second_hand_transaction/get_list?like=0", { page: 1, size: 10, "item_name": this.query.word }, (json) => {
		  if (json.result) {
			var result_second_hand_transaction_item_name = json.result.list;
			result_second_hand_transaction_item_name.map(o => o.title = o['item_name'])
	  			this.result_second_hand_transaction_item_name = result_second_hand_transaction_item_name
		 	}
		});
	},
	/**
	 * 获取item_type
	 */
	get_second_hand_transaction_item_type(){
		this.$get("~/api/second_hand_transaction/get_list?like=0", { page: 1, size: 10, "item_type": this.query.word }, (json) => {
		  if (json.result) {
			var result_second_hand_transaction_item_type = json.result.list;
			result_second_hand_transaction_item_type.map(o => o.title = o['item_type'])
	  			this.result_second_hand_transaction_item_type = result_second_hand_transaction_item_type
		 	}
		});
	},
	/**
	 * 获取transaction_status
	 */
	get_second_hand_transaction_transaction_status(){
		this.$get("~/api/second_hand_transaction/get_list?like=0", { page: 1, size: 10, "transaction_status": this.query.word }, (json) => {
		  if (json.result) {
			var result_second_hand_transaction_transaction_status = json.result.list;
			result_second_hand_transaction_transaction_status.map(o => o.title = o['transaction_status'])
	  			this.result_second_hand_transaction_transaction_status = result_second_hand_transaction_transaction_status
		 	}
		});
	},

  },
  components: { list_result_search },
	created(){
    this.query.word = this.$route.query.word || "";
  },
  mounted() {
	this.get_goods();
	this.get_article();
	this.get_forum();
		this.get_seller_seller_name();
		this.get_seller_store_name();
		this.get_seller_store_location();
		this.get_ordinary_users_user_name();
		this.get_ordinary_users_user_gender();
		this.get_ordinary_users_user_number();
		this.get_shopping_mall_store_name();
		this.get_second_hand_transaction_item_name();
		this.get_second_hand_transaction_item_type();
		this.get_second_hand_transaction_transaction_status();
  },
  watch: {
	$route() {
	  $.push(this.query, this.$route.query);
	  this.get_goods();
	  this.get_article();
	  this.get_forum();
	  this.get_seller_seller_name();
	  this.get_seller_store_name();
	  this.get_seller_store_location();
	  this.get_ordinary_users_user_name();
	  this.get_ordinary_users_user_gender();
	  this.get_ordinary_users_user_number();
	  this.get_shopping_mall_store_name();
	  this.get_second_hand_transaction_item_name();
	  this.get_second_hand_transaction_item_type();
	  this.get_second_hand_transaction_transaction_status();
	},
  },
};
</script>

<style scoped>
.card_search {
  text-align: center;
}
.card_result_search>.title {
  text-align: center;
  padding: 10px 0;
}
</style>
