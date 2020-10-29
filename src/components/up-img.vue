<template>
	<div class="hello">
		<div id="imgArea">
			<img :src="upstyle" class="upstyle" @click="bigImage"/>
			<input type="file" name="url" id="uppic" accept="image/gif,image/jpeg,image/jpg,image/png" @change="changeImage($event)" ref="urlInput" class="uppic">	
		</div>		
		<div class="tips" v-if="flag">上传成功</div>	
	</div>	
</template>

<script>
	
	export default {
	 name: 'hello',
	 data() {
	  return {
	  upstyle: require('../assets/up.png'),
	  flag:false,
	  progress: '',
	  showp: true
	  }
	 },
	 methods:{
		   changeImage(e) {
		   var flag = this.flag
		   var file = e.target.files[0]
		   var reader = new FileReader()
		   var that = this
		   reader.readAsDataURL(file)
		   reader.onload = function(e) {
		    that.upstyle = this.result
			flag = true
			setTimeout( ()=>{
				flag = false
			},1000)
		   }					
		},
		  bigImage(){
			  var box=document.getElementById('imgArea');
			  var  newDiv=document.createElement('div');
			  	 newDiv.className='mark';
			  	 box.appendChild(newDiv);
			  var  newImg=document.createElement('img');
			  	 newImg.className='pic';
			  	 newImg.src=this.upstyle;
			  	 box.appendChild(newImg);
			  var newSpan=document.createElement("span");
			  	newSpan.innerHTML='X';
			  	box.appendChild(newSpan);
							  
			  	newSpan.onclick=function(){
			  		box.removeChild(newDiv);
			  		box.removeChild(newSpan);
			  		box.removeChild(newImg);
			  	}
		  }
	 }
	 }
</script>

<style>
	.uppic {   
	   margin: 0 auto;	   
	}
	 .upstyle {
	   position: absolute;
	   margin-top: 40px;
	   width: 100px;
	   height: 100px;
	   border-style:solid;
	   border-color:#A0A1A7;
	}
	img{width:100%;height:100%;}
	.mark{width:600px;height:400px; background:#000;opacity: 0.5;margin-left: 300px;}
	 /*修改pic的宽高,可调整图片大小*/
	.pic{width:400px;height:380px; position: absolute;left:400px;top:120px;}
	span{position: absolute;width:20px;height:20px;left:800px;top:120px; background: #fff;text-align: center;}
</style>
