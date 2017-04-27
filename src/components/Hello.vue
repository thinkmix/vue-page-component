<template>
	<vue-page-component :maxpage='pagination.maxpage' :page='pagination.page' :size='true' :callback='pageCallback'></vue-page-component>
</template>
<script>
import vuePageComponent from './page.vue';
export default{
	name:'Hello',
	data(){
		return{
			pagination:{
				page:2,//当前显示页
				maxpage:20,//最大页数
				size:false,//true为小，false为大
			}
		}
	},
	components:{
		'vue-page-component':vuePageComponent,
	},
	methods:{
		pageCallback(page){//分页点击回调
			this.pagination.page=page;
		}
	}	
}
</script>
