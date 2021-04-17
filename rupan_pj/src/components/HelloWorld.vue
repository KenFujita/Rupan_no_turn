<template>
	<div>
		<p class="hello">{{word}}!</p>
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
.hello {
	color:#ffffff;
	font-size:20pt;
	font-weight:bold;
	border:1px solid magenta;
	padding:5px 10px 0px 10px;
	margin:10px;
}
</style>
