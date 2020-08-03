<template>
	<view>
		<jyf-parser :html="html" ref="article"></jyf-parser>
	</view>
</template>

<script>
	import jyfParser from "@/components/jyf-parser/jyf-parser";
	export default{
	  // HBuilderX 2.5.5+ 可以通过 easycom 自动引入
	  components: {
		jyfParser
	  },
	  onLoad(options) {
		  if (options.id) {
			  this.id = options.id
		  }
		  
		  
		uni.request({
		    url: 'https://admin.gglaisong.com/notoken/user/article_by_id', //仅为示例，并非真实接口地址。
		    data: {
		        id: this.id
		    },
			method:'POST',
			header:{
				'content-type':'application/x-www-form-urlencoded'
			},
		    success: (res) => {
				this.html = res.data.data.articleContent
		        console.log(res)
				uni.setNavigationBarTitle({
					title: res.data.data.articleTitle
 				})
		    }
		});
	  },
	  data() {
		return {
		  html: '',
		  id: 43
		}
	  }
	}
</script>

<style>
</style>
