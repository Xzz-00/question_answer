<template>
	<view class="content">
		<view class='title'>
			<text>这是一套{{questionTitle}}!</text>
		</view>
		
		<view class="question" v-for="(item,index) in questionList":key="index">
			<view v-if="item.type=='radio'">
				<view  class="question-title">
					<span>{{index+1}}.{{item.label}}</span>
				</view>
				<view class="uni-list">
					<radio-group @change='getQuestionValue'>
					    <label class="uni-list-cell uni-list-cell-pd" v-for="(citem, cindex) in item.options" :key="cindex">
					        <view>
					            <radio :value="citem.value" :checked="cindex === current" class="uni-radio"/>
								<text>{{citem.label}}</text>
					        </view>
					    </label>
					</radio-group>
				</view>
			</view>
		</view>
		
		<view class="result">
			<button @click="setQuestionAnswer">回答完，提交</button>
		</view>
	</view>
</template>

<script>
	export default {
		data() {
			return {
				questionTitle:'',
				questionList:[],
				answerList:[],
				answerValue:[],
				answerLabel:[],
				current:''
			}
		},
		onReady(){
			
		},
		onLoad() {
			 this.getData();
			 
		},
		methods: {
			getData(){
				uni.request({
				    url: 'https://mj.wlear.com/addons/questionnaire/index/questions',
				    success: (res) => {
						this.questionTitle=res.data.data.title;
						this.questionList=res.data.data.questions;
						uni.setNavigationBarTitle({
						    title:res.data.data.title
						});
				    }
				 }); 
			},
			getQuestionValue(e){
				// console.log(e.detail.value);
				// console.log(e.detail);
				this.answerValue.push(e.detail.value);
				// console.log(this.answerValue);
			},
			setQuestionAnswer(){
				this.answerList=this.questionList;
				// console.log(this.answerList);
				for( let i=0;i<this.answerList.length;i++){
					let options=this.answerList[i].options;
					// console.log(options);
					for(let j=0;j<options.length;j++){
						console.log(options[j]);
						if(this.answerValue[i]==options[j].value){
							let label=options[j].label;
							this.answerList[i].options=[
								{
									"label":"",
									"value":""
								}
							]
							this.answerList[i].options.label=label;
							this.answerList[i].options.value=this.answerValue[i];
						}
					}
				}
				console.log(this.answerList);
			}
		}
	}
</script>

<style>
	.content {
		/* display: flex;
		flex-direction: column; */
		width:90%;
		padding: 20rpx 30rpx;
		align-items: center;
		justify-content: center;
	}
	.title{
		font-size: 38rpx;
		padding: 20rpx 20rpx;
	}
	.question-title{
		font-size: 36rpx;
		padding: 5rpx 5rpx;
	}
	.uni-list-cell{
		font-size: 32rpx;
		padding: 2rpx 2rpx;
	}
</style>
