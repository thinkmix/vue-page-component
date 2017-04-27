<template>
	<div class="page-box">
		<ul class="pagination" :class="{paginationSm:size}" v-if="maxpage">
			<li><a @click="handle('prev')"><i class="page-icon-left"></i></a></li>
			<li v-if="pages.isHome"><a @click="handle('home')">首页</a></li>
			<li v-for="todo in pages.left" :class="{active:page==todo}">
				<a @click="handle(0,todo)">{{todo}}</a>
			</li>
			<li v-if="pages.isSpace"><a href="javascript:void(0)">···</a></li>
			<li v-if="pages.isSpace" v-for="todo in pages.right" :class="{active:page==todo}">
				<a @click="handle('index',todo)">{{todo}}</a>
			</li>
			<li><a @click="handle('next')"><i class="page-icon-right"></i></a></li>
		</ul>
		<div v-if="!maxpage" style="text-align:center">没有数据</div>
	</div>
</template>
<script>
export default{
	name:'pagination',
	props:{
		maxpage:Number,
		page:Number,
		size:Boolean,
		callback:Function
	},
	computed:{
		pages(){
			var arr={isHome:!1,isSpace:!1,left:[],right:[]},maxpage=this.maxpage,page=this.page;

			if(maxpage<=8){
		        for(var i=1;i<=maxpage;i++){
		        	arr.left.push(i)		            
		        }
		    }else{
		        if((page!=4&&(page+1)%5==0)||page==5){
		            if(page+7>maxpage){
		                arr.isHome=true;
		                for(var i=maxpage-7;i<=maxpage;i++){
		                    arr.left.push(i)
		                }
		            }else{
		                arr.isHome=true;
		                for(var i=page;i<=page+4;i++){
		                    arr.left.push(i)
		                }
		                arr.isSpace=true;
		                arr.right=[maxpage-1,maxpage];
		            }
		            
		        }else if(page==1){
		        	arr.isHome=true;
	                for(var i=1;i<=5;i++){
	                    arr.left.push(i)
	                }
	                arr.isSpace=true;
	                arr.right=[maxpage-1,maxpage];
		        }else{
		            if(page+7>maxpage){
		            	arr.isHome=true;
		                for(var i=maxpage-7;i<=maxpage;i++){
		                    arr.left.push(i)
		                }
		            }else{
		                arr.isHome=true;
		                var pageL=1;
		                if((page)%5==0){
		                    pageL=page;
		                }else if((page-1)%5==0){
		                    pageL=page-1;
		                }else if((page-2)%5==0){
		                    pageL=page-2;
		                }else if((page-3)%5==0){
		                    pageL=page-3;
		                }
		                pageL==0||page==null ? (pageL=1) : pageL;
		                for(var i=pageL;i<=pageL+4;i++){
		                   arr.left.push(i) 
		                }
		                arr.isSpace=true;
	                	arr.right=[maxpage-1,maxpage];
		            }
		        }
		        
		    }
		    return arr;
		}
	},
	methods:{
		handle(type,page){
			switch(type){
		        case 0:
		            page=page
		            break;
		        case "prev":
		            if(this.page==1) return false;
		            page=this.page-1;
		            break;
		        case "next":
		            if(this.page==this.maxpage) return false;
		            page=this.page+1;
		            break;
		        case "home":
		            page=1
		            break;
		    }
		    this.callback(page);
		}
	}
}
</script>
<style scoped>
*{margin:0;padding:0;}
.page-box{ padding:20px;}
.page-box>.pagination {display:inline-block;padding-left:0;border-radius:4px;list-style: none;user-select:none;-ms-user-select:none;-moz-user-select:none;-webkit-user-select:none;}
.page-box>.pagination>li {display:inline;}
.page-box>.pagination>li>a{position:relative;float:left;display: inline-block;height: 36px;line-height: 36px; color: #2e3e4e; padding:0 12px;margin-left:-1px;text-decoration:none;background-color:#ffffff;border:1px solid #dddddd;cursor: pointer;box-sizing: border-box;-webkit-box-sizing: border-box;-moz-box-sizing: border-box;font-size: 14px;}
.page-box>.pagination>li:first-child>a{margin-left:0;padding: 0;border-bottom-left-radius:4px;border-top-left-radius:4px;}
.page-box>.pagination>li:last-child>a{border-top-right-radius:4px;border-bottom-right-radius:4px;padding: 0;}
.page-box>.pagination>li>a:hover, .page-box>.pagination>li>a:focus{background-color:#eeeeee;}
.page-box>.pagination>li>a>i{ display: inline-block; padding:0px 15px; height: 35px;vertical-align: top;}
.page-box>.pagination>.active>a, .page-box>.pagination>.active>a:hover, .page-box>.pagination>.active>a:focus{z-index:2;color:#ffffff;cursor:default;background-color:#20a0ff;border-color:#20a0ff;}
.page-box>.paginationSm>li>a{padding:0px 10px;font-size:13px;height: 30px;line-height: 30px;  cursor: pointer;}
.page-box>.paginationSm>li:first-child>a{border-bottom-left-radius:3px;border-top-left-radius:3px;}
.page-box>.paginationSm>li:last-child>a{border-top-right-radius:3px;border-bottom-right-radius:3px;}
.page-box>.paginationSm>li>a>i{padding:0px 15px; height: 28px;}
.page-icon-left{ background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAA4ElEQVRYR93XwQ2DMAwFUNxd0g2YpzBZwzplA7JLXXyoVKFI/MQ/RCoXDkT8F4NskKHzIZ3zh/8DhDBOIqLb9lqQ6lIrcA9j3MMfFvxWnVNa4xmCBvgNvxxwDFfVZUvrdLZ7u+6ugCfcDfCGuwCM8GoAK7wKwAwvBrDDiwAtwmFAq3AIYL39JvL8NpWSJkNpRBlA3LvcjNwcWQN1wswjoCEggO2kFQIGtEIUAVogigFsRBWAiagGsBAuAAPhBngRFEAOcflX8RHRBWAImx12Rv4JoGmIDBTPGto7UIvoDvgAtCLJIQrOGCMAAAAASUVORK5CYII=);background-repeat: no-repeat;background-position: center;background-size:50%; }
.page-icon-right{background-image: url(data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAYAAABzenr0AAAAGXRFWHRTb2Z0d2FyZQBBZG9iZSBJbWFnZVJlYWR5ccllPAAAAyJpVFh0WE1MOmNvbS5hZG9iZS54bXAAAAAAADw/eHBhY2tldCBiZWdpbj0i77u/IiBpZD0iVzVNME1wQ2VoaUh6cmVTek5UY3prYzlkIj8+IDx4OnhtcG1ldGEgeG1sbnM6eD0iYWRvYmU6bnM6bWV0YS8iIHg6eG1wdGs9IkFkb2JlIFhNUCBDb3JlIDUuMy1jMDExIDY2LjE0NTY2MSwgMjAxMi8wMi8wNi0xNDo1NjoyNyAgICAgICAgIj4gPHJkZjpSREYgeG1sbnM6cmRmPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5LzAyLzIyLXJkZi1zeW50YXgtbnMjIj4gPHJkZjpEZXNjcmlwdGlvbiByZGY6YWJvdXQ9IiIgeG1sbnM6eG1wPSJodHRwOi8vbnMuYWRvYmUuY29tL3hhcC8xLjAvIiB4bWxuczp4bXBNTT0iaHR0cDovL25zLmFkb2JlLmNvbS94YXAvMS4wL21tLyIgeG1sbnM6c3RSZWY9Imh0dHA6Ly9ucy5hZG9iZS5jb20veGFwLzEuMC9zVHlwZS9SZXNvdXJjZVJlZiMiIHhtcDpDcmVhdG9yVG9vbD0iQWRvYmUgUGhvdG9zaG9wIENTNiAoV2luZG93cykiIHhtcE1NOkluc3RhbmNlSUQ9InhtcC5paWQ6Rjc3MUIyQkE5MUUxMTFFNkI1QTRBQzZDOUExRkQxQUEiIHhtcE1NOkRvY3VtZW50SUQ9InhtcC5kaWQ6Rjc3MUIyQkI5MUUxMTFFNkI1QTRBQzZDOUExRkQxQUEiPiA8eG1wTU06RGVyaXZlZEZyb20gc3RSZWY6aW5zdGFuY2VJRD0ieG1wLmlpZDpGNzcxQjJCODkxRTExMUU2QjVBNEFDNkM5QTFGRDFBQSIgc3RSZWY6ZG9jdW1lbnRJRD0ieG1wLmRpZDpGNzcxQjJCOTkxRTExMUU2QjVBNEFDNkM5QTFGRDFBQSIvPiA8L3JkZjpEZXNjcmlwdGlvbj4gPC9yZGY6UkRGPiA8L3g6eG1wbWV0YT4gPD94cGFja2V0IGVuZD0iciI/PnkX7LIAAADiSURBVHja7JfRDcMgDEQx6iqwQbpO0slK58kGYRiKK1XqR6UafI5/ihSJD6R7nJEvptZa8FwxOC93gIvkEBGFlJaN97XuZUTgV4lFDrB4JLrzl9NSXEvQ3ViRECIAtr1bWSwgxA4cdb9ZQAyVwAJi+A2gIab6ABJiuhGhIFSdEAGhbsVaCEgWaCBgYfQN4p0fZ6YhucUxW863/kjBhyQ5o5V4L8l2igMacTWAVlwFgBCfBkCJTwEgxYcB0OJDABbiQ3/FFuJTbwAp/urdktmQB5Ocr2s/S+jBhP7DqTfAU4ABAMYPxXxvY41GAAAAAElFTkSuQmCC);background-repeat:  no-repeat;background-position: center;background-size:50%;}

</style>