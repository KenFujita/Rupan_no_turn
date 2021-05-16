<template>
	<div class="haichi">
		<h2 class="description">ルパンのアレを再現するページ</h2>
		<div>
			<input type="text"
			v-bind:disabled="isDis"
			@keydown.enter="isEnter"
			@compositionstart="composing=true"
			@compositionend="composing=false"
			v-on:input="write" 
			v-model="word"
			>
			<p style="color: white">サブタイっぽいのを入力してみよう！</p>
		</div>
		<textarea readonly class="hello" v-bind:class="{zoomin: isZoom}" v-model="word"></textarea>
	</div>
</template>

<script>

export default {
  name: 'HelloWorld',
  data:function(){
	return {
		keyPushpath:require("@/assets/typewriter.mp3"),
		enterPushpath:require("@/assets/rupanIcatch.wav"),
		keyPush:new Audio(''),
		enterPush:new Audio(''),
		word:'',
		composing:false,
		isDis:false,
		isZoom:false
	};
  },
  mounted: function(){
    this.keyPush.src = this.keyPushpath;
    this.keyPush.load();

    this.enterPush.src = this.enterPushpath;
    this.enterPush.load();
  },
  methods:{
	write:function(event) {
		if(event.inputType == undefined || event.inputType === "deleteContentBackward"){
			return;
		} else {
			this.keyPush.currentTime = 0;
			this.keyPush.play();
		}
		this.word = event.target.value;
	},
	isEnter:async function() {
		if(!this.composing){
			this.isDis = this.isZoom = true;
			this.enterPush.currentTime = 0.5;
			this.enterPush.play();
			const res = await this.donotWrite(this.enterPush);
			console.log(res);
			this.isDis = this.isZoom = false;
			this.word = '';
		}
	},
	donotWrite:function(aud) {
		return new Promise((resolve) => {
			aud.addEventListener('ended',()=>{
				resolve('done');
			});
		});
	}
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.haichi {
	width: 100%
}

.hello {
	color:#ffffff;
	background-color: black;
	font-size:128pt;
	font-weight:bold;
	text-align: center;
	border: none;
	box-sizing: border-box;
	padding:10px 10px 10px 10px;
	margin: 10px 0px 10px 0px;
	width: 100%;
	height: calc(1.2em * 3);
	line-height: 1.2;
}

.description {
	width: 480px;
	margin: 0 auto;
	color: red;
	text-align: center;
}

.zoomin {
	animation: zoomIn 0.8s cubic-bezier(0.25, 1, 0.5, 1) 1 forwards;
}
 
@keyframes zoomIn {
	0% {
		transform: scale(0.8);
		opacity: 0;
	}
	100% {
		opacity: 1;
		transform: scale(1);
	}
}
</style>
