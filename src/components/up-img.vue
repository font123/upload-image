<template>
	<div class="hello ">
		<div id="imgArea" class = "col-md-offset-4 col-lg-offset-4col-xl-offset-4">
			<form id="fileForm" enctype="multipart/form-data" class="form-horizontal " >
			  <div class="col-6" style="margin:10px 25%;padding: 0">
			     <input type="file" name="url" id="uppic" multiple="multiple" accept="image/gif,image/jpeg,image/jpg,image/png" @change="changeImage($event,0)" ref="urlInput" class="uppic">	
			    </div>
			</form>		
			<div class="col-6" >
			    <img :src="upStyle" class="img-rounded upstyle" @click="bigImage(this.src, this.width, this.height);" data-target="#myModal" data-toggle="modal">
			</div>		 
			<div class="col-6">
			<div class="progress">
			  <div id="progressactive" class="progress-bar progress-bar-striped active"  role="progressbar" style="width: 0%;" aria-valuenow="25" aria-valuemin="0" aria-valuemax="100">0%</div>
			</div>
			</div>
			<!-- 模态框（Modal） -->
			<div class="modal fade" id="myModal" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
			    <div class="modal-dialog">
					<div class="modal-header">
						<button type="button" class="close" data-dismiss="modal" aria-label="Close">
						  <span aria-hidden="true">&times;</span>
						</button>
					</div>
			        <div class="modal-body" align="center">
						<div class="show-image">
							<img :src="upStyle" id="image ":style="{transform:'scale('+mulTiples+')'}">
						</div>
						
						<div class="look-image-footer">
						    <div class="enlargement" @click="magnify">
						        <span><img src="../assets/big.png"></span>
						    </div>
						    <div class="shrink" @click="shrink">
						        <span><img src="../assets/small.png"></span>
						    </div>
						</div>
			        </div>
					
			    </div>	
			</div>
		</div>
	</div>	
</template>
 
<script>	
	export default {
	 name: 'hello',
	 data() {
	  return {
	  upStyle: require('../assets/up.png'),
	  mulTiples: 1,       // 放大或者缩小
	  }
	 },
	 methods:{
		   changeImage(e,key) {
			for(var i=0;i<e.target.files.length;i++) {  //循环获取上传个文件
				 var upFile = e.target.files[i]
			}
		   var fileReader = new FileReader()
		   var that = this
		   fileReader.readAsDataURL(upFile)
		   fileReader.onload = function(e) {
		    that.upStyle = this.result
		   }
		let upImg = event.target.files[0];
		let formData = new FormData();
		var baseUrl = '/upimg'
		var progressBar = document.getElementById("progressactive");//获取进度条对象
		      formData.append('file',upImg);
		      this.$axios.post(baseUrl,formData,{
		        headers:{'Content-Type':'multipart/form-data'},
		        withCredentials:true,
		        onUploadProgress: progressEvent => {
					var completePr = (progressEvent.loaded / progressEvent.total * 100 | 0)+"%"
					progressBar.style.width = completePr;
					progressBar.innerHTML = completePr;				  
		        }
						  
		      }).then(response => {  
		        let url = response.data.data.file_url;//上传成功的返回url
		        if (key === 0) {
		          this.upStyle = url;
		        } 
		     	console.log('上传成功')
		      }).catch(error => {  
		        console.log('上传失败',error)
		      })
		},
		   //查看大图
		  bigImage(src, width, height){
				$('#myModal').on('show.bs.modal', function () {
					var myModal = $(this);
					myModal.find('.modal-dialog').css({'margin-left':(document.body.clientWidth - width*1.5)/2 + 'px'})
					myModal.find('.modal-body #image').attr("src", src)
						.attr("width", width*1.5)
						.attr("height", height*1.5);
					});
			},
			
			 // 放大
			magnify() {
				if(this.mulTiples >= 10){
					return
				}
				this.mulTiples += 0.25;		
			},
			// 缩小
			shrink() {
				if(this.mulTiples <= 0){
					return
				}
				this.mulTiples -= 0.25;	
			}
		  
		}
	 }
</script>

<style>
	#imgArea{
		text-align: center;
		width: 400px;
		height: 200px;
		border-style:solid;
		border-color:#A0A1A7;
	}
	.upstyle {   
	   width: 100px;
	   height: 100px;
	   border-style:solid;
	   border-color:#A0A1A7;
	}
	img{
		width:100%;
		height:100%;
	}
	.progress{
		margin-top: 10px;
	}
	.show-image{
		overflow: hidden;
	}
	.look-image-footer img{
		width: 32px;
		height: 32px;	
	}
	.look-image-footer {
		 display: flex;
		 height: 110px;
		 line-height: 110px;
		 >div {
			 width: 25%;
			 position: relative;
			 &::after {
				 content: "";
				 position: absolute;
				 top: 0;
				 right: 0;
				 width: 1px;
				 height: 100%;
				 background-color: #c5c5c5;
			 }
			 &:last-child {
				 &::after {
					 display: none;
				 }
			 }
		 }
	}
	 	    
</style>
