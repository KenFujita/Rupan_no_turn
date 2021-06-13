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
			<span style="color: white">←  サブタイっぽいのを入力してみよう！(25文字以内で)</span>
		</div>
		<div class="hello">
			<span> {{ isDis ? word : word.substr(-1,1) }} </span>
		</div>
	</div>
</template>

<script>

export default {
  name: 'HelloWorld',
  data:function(){
	return {
		keyPushpath:"https://rpnassets.s3.ap-northeast-1.amazonaws.com/typewriter.mp3",
		enterPushpath:"https://rpnassets.s3.ap-northeast-1.amazonaws.com/rupansubtitle.wav",
		keyPush:new Audio(''),
		enterPush:new Audio(''),
		word:'',
		composing:false,
		isDis:false
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
		console.log(this.composing);
		if(!this.composing || (this.composing && this.word.length > 25)){
			this.isDis = true;
			this.enterPush.currentTime = 0;
			this.enterPush.play();
			const res = await this.donotWrite(this.enterPush);
			console.log(res);
			this.isDis = false;
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
<style src="./rupan.css" scoped></style>
