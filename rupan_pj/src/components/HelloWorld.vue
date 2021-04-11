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
    var keyPush = new Audio('');
    var enterPush = new Audio('');

    keyPush.src = "../typewriter.mp3";
    keyPush.load();

    enterPush.src = "../rupanIcatch.wav";
    enterPush.load();

export default {
  name: 'HelloWorld',
  data:function(){
	return {
		word:'',
		composing:false,
		isDis:false,
	};
  },
  methods:{
	write:function(event) {
		if(event.inputType == undefined || event.inputType === "deleteContentBackward"){
			return;
		} else {
			keyPush.currentTime = 0;
			keyPush.play();
		}
		this.word = event.target.value;
	},
	isEnter:async function() {
		if(!this.composing){
			this.isDis = true;
			this.word = '';
			enterPush.currentTime = 0.5;
			enterPush.play();
			const res = await this.donotWrite(enterPush);
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
	font-size:20pt;
	font-weight:bold;
	border:1px solid magenta;
	padding:5px 10px 0px 10px;
	margin:10px;
}
</style>
