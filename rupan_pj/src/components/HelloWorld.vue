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
		</div>
		<textarea readonly class="hello" v-model="word"></textarea>
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
			this.isDis = true;
			this.word = '';
			this.enterPush.currentTime = 0.5;
			this.enterPush.play();
			const res = await this.donotWrite(this.enterPush);
			console.log(res);
			this.isDis = false;
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
	border: 1px solid darkcyan;
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
	color:#ffffff;
	text-align: center;
}
</style>
