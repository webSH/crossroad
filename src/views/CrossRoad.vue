<template>
	<div class="taL">
		<h2>traffic light</h2>
		<section v-if="SN.A && EW.A">
			<p>
				<span class="dpIB w4">南北↕:</span>
				<label for="SN_duration_A">直行绿灯时间 <b class="cG">↑</b></label><input type="tel" maxlength="2" id="SN_duration_A" v-model.number="SN.duration_A" />（秒）
				<label for="SN_duration_L">左转绿灯时间 <b class="cG">←</b></label><input type="tel" maxlength="2" id="SN_duration_L" v-model.number="SN.duration_L" />（秒）
				<label for="SN_duration_flash">绿灯闪烁时间 <b class="cG">¤</b></label><input type="tel" maxlength="2" id="SN_duration_flash" v-model.number="SN.duration_flash" />（秒）
				<label for="SN_duration_y">黄灯时间 <b class="cY">●</b></label><input type="tel" maxlength="2" id="SN_duration_y" v-model.number="SN.duration_y" />（秒）
				<label for="SN_delay_r">红灯后延时 <b class="cR">§</b></label><input type="tel" maxlength="2" id="SN_delay_r" v-model.number="SN.delay_r" />（秒）
			</p>
			<p>
				<span class="dpIB w4">东西 ↔:</span>
				<label for="EW_duration_A">直行绿灯时间 <b class="cG">↑</b></label><input type="tel" maxlength="2" id="EW_duration_A" v-model.number="EW.duration_A" />（秒）
				<label for="EW_duration_L">左转绿灯时间 <b class="cG">←</b></label><input type="tel" maxlength="2" id="EW_duration_L" v-model.number="EW.duration_L" />（秒）
				<label for="EW_duration_flash">绿灯闪烁时间 <b class="cG">¤</b></label><input type="tel" maxlength="2" id="EW_duration_flash" v-model.number="EW.duration_flash" />（秒）
				<label for="EW_duration_y">黄灯时间 <b class="cY">●</b></label><input type="tel" maxlength="2" id="EW_duration_y" v-model.number="EW.duration_y" />（秒）
				<label for="EW_delay_r">红灯后延时 <b class="cR">§</b></label><input type="tel" maxlength="2" id="EW_delay_r" v-model.number="EW.delay_r" />（秒）
			</p>
		</section>
		<section class="mt1">
			<p class="mr1_sub">
				<button class="bgGreen" @click="status=='stop'?init():(status=='run'?pause():goOn())">{{status=='stop'?'START':(status=='run'?'PAUSE':'CONTINUE')}}</button>
				<button class="bgRed" @click="stop()">STOP</button>
				<button class="bgGreen" @click="initDuration()">RESET DURATION</button>
			</p>
		</section>
		<div v-if="SN.A && EW.A" class="space">
			<div class="WN">

			</div>
			<div class="EN">
				
			</div>
			<div class="WS">

			</div>
			<div class="ES">
				
			</div>
			<div class="road road_SN">
				<div class="down">
					<div class="in">
						<p class="line a" :class="{'opBgGreen':SN.A['g'].on,'opBgFlash':SN.A['g'].flash,'opBgYellow':SN.A['y'].on,'opBgRed':SN.A['r'].on}"></p>
						<p class="line a" :class="{'opBgGreen':SN.A['g'].on,'opBgFlash':SN.A['g'].flash,'opBgYellow':SN.A['y'].on,'opBgRed':SN.A['r'].on}"></p>
						<p class="line l" :class="{'opBgGreen':SN.L['g'].on,'opBgFlash':SN.L['g'].flash,'opBgYellow':SN.L['y'].on,'opBgRed':SN.L['r'].on}"></p>
					</div>
					<div class="out">
						<p class="line a"></p>
						<p class="line a"></p>
						<p class="line a"></p>
					</div>
				</div>
				<div class="up">
					<div class="in">
						<p class="line l" :class="{'opBgGreen':SN.L['g'].on,'opBgFlash':SN.L['g'].flash,'opBgYellow':SN.L['y'].on,'opBgRed':SN.L['r'].on}"></p>
						<p class="line a" :class="{'opBgGreen':SN.A['g'].on,'opBgFlash':SN.A['g'].flash,'opBgYellow':SN.A['y'].on,'opBgRed':SN.A['r'].on}"></p>
						<p class="line a" :class="{'opBgGreen':SN.A['g'].on,'opBgFlash':SN.A['g'].flash,'opBgYellow':SN.A['y'].on,'opBgRed':SN.A['r'].on}"></p>
					</div>
					<div class="out">
						<p class="line a"></p>
						<p class="line a"></p>
						<p class="line a"></p>
					</div>
				</div>
			</div>
			<div class="road road_EW">
				<div class="left">
					<div class="in">
						<p class="line a" :class="{'opBgGreen':EW.A['g'].on,'opBgFlash':EW.A['g'].flash,'opBgYellow':EW.A['y'].on,'opBgRed':EW.A['r'].on}"></p>
						<p class="line a" :class="{'opBgGreen':EW.A['g'].on,'opBgFlash':EW.A['g'].flash,'opBgYellow':EW.A['y'].on,'opBgRed':EW.A['r'].on}"></p>
						<p class="line l" :class="{'opBgGreen':EW.L['g'].on,'opBgFlash':EW.L['g'].flash,'opBgYellow':EW.L['y'].on,'opBgRed':EW.L['r'].on}"></p>
					</div>
					<div class="out">
						<p class="line a"></p>
						<p class="line a"></p>
						<p class="line a"></p>
					</div>
				</div>
				<div class="right">
					<div class="in">
						<p class="line l" :class="{'opBgGreen':EW.L['g'].on,'opBgFlash':EW.L['g'].flash,'opBgYellow':EW.L['y'].on,'opBgRed':EW.L['r'].on}"></p>
						<p class="line a" :class="{'opBgGreen':EW.A['g'].on,'opBgFlash':EW.A['g'].flash,'opBgYellow':EW.A['y'].on,'opBgRed':EW.A['r'].on}"></p>
						<p class="line a" :class="{'opBgGreen':EW.A['g'].on,'opBgFlash':EW.A['g'].flash,'opBgYellow':EW.A['y'].on,'opBgRed':EW.A['r'].on}"></p>
					</div>
					<div class="out">
						<p class="line a"></p>
						<p class="line a"></p>
						<p class="line a"></p>
					</div>
				</div>
			</div>
			<div class="light light_SN">
				<p class="turnL">
					<span v-for="ltL in SN.L" :class="[ltL.class,{'on':ltL.on},{'flash':ltL.flash}]">
						<!-- <Icon v-if="ltL.class=='r' || ltL.class=='g'" name="left" class="ico w1x h1x" style="width:80%;height:80%;" /> -->
					</span>
				</p>
				<p class="turnA">
					<span :a="ltA.on" v-for="ltA in SN.A" :class="[ltA.class,{'on':ltA.on},{'flash':ltA.flash}]"></span>
				</p>
				<p class="countdown"><b class="LED" :class="{'cG':SN.A['g'].on || SN.L['g'].on, 'cY':SN.A['y'].on || SN.L['y'].on, 'cR':SN.A['r'].on && SN.L['r'].on}">{{textTime(SN.countdown)}}</b></p>
			</div>
			<div class="light light_EW">
				<p class="turnL">
					<span v-for="ltL in EW.L" :class="[ltL.class,{'on':ltL.on},{'flash':ltL.flash}]">
						<!-- <Icon v-if="ltL.class=='r' || ltL.class=='g'" name="left" class="ico w1x h1x" style="width:80%;height:80%;" /> -->
					</span>
				</p>
				<p class="turnA">
					<span v-for="ltA in EW.A" :class="[ltA.class,{'on':ltA.on},{'flash':ltA.flash}]"></span>
				</p>
				<p class="countdown"><b class="LED" :class="{'cG':EW.A['g'].on || EW.L['g'].on, 'cY':EW.A['y'].on || EW.L['y'].on, 'cR':EW.A['r'].on && EW.L['r'].on}">{{textTime(EW.countdown)}}</b></p>
			</div>
		</div>

	</div>
</template>
<script>
	import {deepCopy} from '@/assets/js/main'
	export default {
		name: 'crossRoad',
		components: {

		},
		data() {
			return {
				SN_init:{
					L:{
						r: {class: 'r', on: false, flash: false},
						y: {class: 'y', on: false, flash: false},
						g: {class: 'g', on: false, flash: false},
					},
					A:{
						r: {class: 'r', on: false, flash: false},
						y: {class: 'y', on: false, flash: false},
						g: {class: 'g', on: false, flash: false},
					},
					countdownTimer: null,
					countdown: 0,
					duration_L: 5,
					duration_A: 10,
					duration_y: 3,
					duration_flash: 3,
					delay_r: 2 // 红灯延时(秒)
				},
				EW_init:{
					L:{
						r: {class: 'r', on: false, flash: false},
						y: {class: 'y', on: false, flash: false},
						g: {class: 'g', on: false, flash: false},
					},
					A:{
						r: {class: 'r', on: false, flash: false},
						y: {class: 'y', on: false, flash: false},
						g: {class: 'g', on: false, flash: false},
					},
					countdownTimer: null,
					countdown: 0,
					duration_L: 5,
					duration_A: 10,
					duration_y: 3,
					duration_flash: 3,
					delay_r: 2 // 红灯延时(秒)
				},
				SN_setup:{},
				EW_setup:{},
				SN:{},
				EW:{},
				status: 'stop',
				statusArray: ['SN_A_g','SN_A_flash','SN_A_y','SN_L_g','SN_L_flash','SN_L_y','SN_switch',
								'EW_A_g','EW_A_flash','EW_A_y','EW_L_g','EW_L_flash','EW_L_y','EW_switch'],
				curStatus:'',
				runTime: null,
			}
		},
		methods: {
			/* 
			init_directionObj_on 第一组
			init_directionObj_off 第二组
			*/
			init(init_directionObj_on=this.SN, init_directionObj_off=this.EW){
				// this.SN = deepCopy(this.SN_setup); //将设置信息给予当前信号机
				// this.EW = deepCopy(this.EW_setup);
				// init_directionObj_on = this.SN;
				// init_directionObj_off = this.EW
				console.log('===Go')
				let $this = this;
				this.status = 'run';
				if (this.runTime) {
					clearTimeout(this.runTime)
				}
				function Countdown(directionObj, duration){	//倒计时计算 directionObj:[南北|东西], duration:传秒
					if (directionObj.countdownTimer) {
						clearInterval(directionObj.countdownTimer)
					}
					directionObj.countdown = duration;
					directionObj.countdownTimer = setInterval(() => { //轮询 → 显示倒计时
						console.log('setInterval')
						if (directionObj.countdown > 0){
							directionObj.countdown --;
						}
					}, 1*1000);
					console.log(directionObj.countdownTimer)
				}

				let i_status = 0;
				function lightDuration(duration) {
					if (i_status == $this.statusArray.length){
						i_status = 0;
					}
					$this.curStatus = $this.statusArray[i_status++];
					return new Promise((resolve,reject)=>{
						$this.runTime = setTimeout(resolve, duration*1000);
					})
				}

				function lightStatusInit(ON,OFF) { //初始化
					// ON: 通行方↓
					ON.A['r'].on = false;
					ON.A['y'].on = false;
					ON.A['g'].on = true;
					ON.A['g'].flash = false;

					ON.L['r'].on = true;
					ON.L['y'].on = false;
					ON.L['g'].on = false;
					ON.L['g'].flash = false;

					// OFF: 等待方↓
					OFF.A['r'].on = true;
					OFF.A['y'].on = false;
					OFF.A['g'].on = false;
					OFF.A['g'].flash = false;

					OFF.L['r'].on = true;
					OFF.L['y'].on = false;
					OFF.L['g'].on = false;
					OFF.L['g'].flash = false;

					Countdown(ON, ON.duration_A); //通行方倒计时
					Countdown(OFF, ON.duration_A + ON.duration_L + ON.duration_y*2 + ON.delay_r); //等待方倒计时(通行方 前+左+黄*2+红灯延时)
				}


				function flashOn(flashObj){	//闪
					flashObj['g'].flash = true;
				}
				function flashOff(flashObj,ON){ //闪-off
					Countdown(ON, ON.duration_y);
					flashObj['g'].flash = false;	//闪-off
					flashObj['g'].on = false;	//绿-off
					flashObj['y'].on = true;	//黄
				}
				function yOff(curLightGroup, nextLightGroup, curDirection, nextDirection, nextDuration, delay_r){ //黄-off(下组绿)
				/*	curLightGroup: 当前（绿灯）组,
					nextLightGroup: 下一（红灯）组,
					nextDirection: 切换后方向组,
					nextDuration: 切换后方向组倒计时,
					delay_r: 绿灯组红灯延时  */
					curLightGroup['y'].on = false;	//1st-黄-off
					curLightGroup['r'].on = true;	//1st-红
					function switchGroup(){ //换组
						nextLightGroup['g'].on = true; //2en-绿
						nextLightGroup['r'].on = false; //2en-红-off
						Countdown(nextDirection, nextDuration);	//2en-倒计时
					}
					if (delay_r){	//有红灯延时(换向)
						Countdown(curDirection, curDirection.delay_r + nextDirection.duration_A + nextDirection.duration_L + nextDirection.duration_y*2 + nextDirection.delay_r); //1st-(红)倒计时
						return lightDuration(delay_r).then(()=>{
							if ($this.status=='run') {
								console.log('===换向')
								switchGroup();
								rUN(nextDirection, curDirection); //换向
							}
						})
					}else{ //同向换组
						switchGroup();
					}
				}
				
				function rUN(directionObj_on, directionObj_off){
					return Promise.resolve()
						.then(()=>{console.log('1st-前绿')	//1st-前绿
							return lightDuration(directionObj_on.duration_A - directionObj_on.duration_flash);
						}).then(()=>{console.log('1st-前闪')	//1st-前闪
							flashOn(directionObj_on.A);
							return lightDuration(directionObj_on.duration_flash);
						}).then(()=>{console.log('1st-前闪-off')	//1st-前闪-off
							flashOff(directionObj_on.A, directionObj_on);
							return lightDuration(directionObj_on.duration_y);
						}).then(()=>{console.log('1st-前黄-off') //1st-前黄-off
							yOff(directionObj_on.A, directionObj_on.L, directionObj_on, directionObj_on, directionObj_on.duration_L);
							return lightDuration(directionObj_on.duration_L - directionObj_on.duration_flash);
						}).then(()=>{console.log('1st-左闪')	//1st-左闪
							flashOn(directionObj_on.L);
							return lightDuration(directionObj_on.duration_flash);
						}).then(()=>{console.log('1st-左闪-off')	//1st-左闪-off
							flashOff(directionObj_on.L, directionObj_on);
							return lightDuration(directionObj_on.duration_y)
						}).then(()=>{console.log('1st-左黄-off')	//1st-左黄-off (换向在 yOff 中延时 directionObj_on.delay_r 完成)
							yOff(directionObj_on.L, directionObj_off.A, directionObj_on, directionObj_off, directionObj_off.duration_A, directionObj_on.delay_r)
						})
				}
				lightStatusInit(init_directionObj_on, init_directionObj_off); //初始化红绿
				rUN(init_directionObj_on, init_directionObj_off) //开始
			},
			initDuration(){ //重置时间
				{
					let {duration_L, duration_A, duration_y, duration_flash, delay_r} = this.SN_init;
					this.SN.duration_L = duration_L;
					this.SN.duration_A = duration_A;
					this.SN.duration_y = duration_y;
					this.SN.duration_flash = duration_flash;
					this.SN.delay_r = delay_r;
				}
				{
					let {duration_L, duration_A, duration_y, duration_flash, delay_r} = this.EW_init;
					this.EW.duration_L = duration_L;
					this.EW.duration_A = duration_A;
					this.EW.duration_y = duration_y;
					this.EW.duration_flash = duration_flash;
					this.EW.delay_r = delay_r;
				}
			},
			initLight(){ //生成信号机
				this.SN = deepCopy(this.SN_init);
				this.EW = deepCopy(this.EW_init);
			},
			clearAll(){
				if (this.runTime) {
					clearTimeout(this.runTime)
				}
				if (this.SN.countdownTimer) {
					clearInterval(this.SN.countdownTimer)
				}
				if (this.EW.countdownTimer) {
					clearInterval(this.EW.countdownTimer)
				}
			},
			goOn(){
				
			},
			pause(){
				console.log('===Pause')
				console.log(this.curStatus+'/n',this.SN.countdown,this.EW.countdown)
				this.status = 'pause';
				this.clearAll();
			},
			stop(){
				console.log('===Stop')
				this.status = 'stop';
				this.clearAll();
				this.initLight();	//生成信号机
			},
			textTime(number){ //固定两位显示
				let text = ('0' + number).substr(-2);
				return text;
			}
		},
		mounted() {
			this.initLight();	//生成信号机
		}
	}
</script>
<style lang="scss" scoped>
$green: #3a3;
$red: #e33;
$yellow: #f90;
$yellow_light: #fd0;

$road_width:10%;

$road_opcity:.1;

*{box-sizing: border-box;}
label{margin-right:.5em;}
input{width: 3em;}
p{margin: 0;}
.cG{color:$green}
.cY{color:$yellow}
.cR{color:$red}
.opBgGreen{background:rgba(51,170,51,$road_opcity)}
.opBgYellow{background:rgba(255,253,0,$road_opcity)}
.opBgRed{background:rgba(238,51,51,$road_opcity)}
.opBgFlash{animation:flash 1s linear infinite;}
.space{
	position: relative;
	width: 80%;
	height: 800px;
	margin: 10% auto;
	background: #eee;
}
.WN,.EN,.WS,.ES{
	position: absolute;
	width: (100% - $road_width)/2;
	height: (100% - $road_width)/2;
	background: rgb(230, 230, 230);
	border:.2em solid rgb(12, 105, 59);
}
.WN{border-top:0;border-left:0;left: 0;border-radius: 0 0 7% 0;}
.EN{border-top:0;border-right:0;right: 0;border-radius: 0 0 0 7%;}
.WS{border-bottom:0;border-left:0;bottom: 0;left: 0;border-radius: 0 7% 0 0;}
.ES{border-bottom:0;border-right:0;bottom: 0;right: 0;border-radius: 7% 0 0 0;}
.road{position: absolute;}
.down,.up{width: 50%;display: inline-block;position: relative;}
.road_SN{left: (100% - $road_width)/2;width: $road_width;height: 100%;
	.line{width: 33.33%;height:100%;display: inline-block;}
	.down,.up{height: 100%;
		.in,.out{position: absolute; width:100%; height: (100% - $road_width*1.5)/2;}
	}
	.down{top: 0;padding-right:1px;
		.line{border-right:2px solid #fff}
		.in{top: 0;border-right: 2px solid $yellow_light;
			&>p:last-child{border-right: 0;}
		}
		.out{bottom: 0;border-right: 2px solid $yellow_light;
			&>p:last-child{border-right: 0;}
		}
	}
	.up{bottom: 0;padding-left:1px;
		.line{border-left:2px solid #fff}
		.in{bottom: 0;border-left: 2px solid $yellow_light;
			&>p:first-child{border-left: 0;}
		}
		.out{top: 0;border-left: 2px solid $yellow_light;
			&>p:first-child{border-left: 0;}
		}
	}
}
.right,.left{height: 50%;position: relative;}
.road_EW{
	top: (100% - $road_width)/2;width: 100%;height: $road_width;
	.line{width: 100%;height: 33.33%;}
	.right,.left{width: 100%;
		.in,.out{position: absolute; width: (100% - $road_width*1.5)/2; height: 100%;}
	}
	.right{left: 0;padding-top:1px;
		.line{border-top:2px solid #fff}
		.in{left: 0;border-top: 2px solid $yellow_light;
			&>p:first-child{border-top: 0;}
		}
		.out{right: 0;border-top: 2px solid $yellow_light;
			&>p:first-child{border-top: 0;}
		}
	}
	.left{right: 0;padding-bottom:1px;
		.line{border-bottom:2px solid #fff}
		.in{right: 0;border-bottom: 2px solid $yellow_light;
			&>p:last-child{border-bottom: 0;}
		}
		.out{left: 0;border-bottom: 2px solid $yellow_light;
			&>p:last-child{border-bottom: 0;}
		}
	}
}
.light{position:absolute;padding: .5em;width: 30%;height: 2em;background: rgb(168, 168, 168);border-radius:2px;
	>p{width:1.8em;padding:.5em .4em;background:#333;border-radius:1em;display:inline-block;margin:-1.5em 1em 0 0;;
		span{
			display: block;
			height: 1em;
			width: 1em;
			border-radius: 50%;
			background: rgb(18, 18, 18);
			border:2px solid #666;
			margin:0 0 .1em 0;
			&::after{content:'';display: block;height: 100%;border-radius: 50%;}
			&.r.on::after{background: $red;}
			&.y.on::after{background: $yellow;}
			&.g.on::after{background: $green;}
			&.flash::after{animation:flash 1s linear infinite;}
		}
		// &.turnL{
		// 	.r.on{background: rgb(18, 18, 18);
		// 		svg{color: #e33;}
		// 	}
		// 	.g.on{background: rgb(18, 18, 18);
		// 		svg{color: #3a3;}
		// 	}
		// }
	}
	
}
.light_SN{
	right: 5%;top: 30%;
}
.light_EW{
	left: 5%;top: 30%;
}
@keyframes flash{
	1%{opacity:0;}
	40%{opacity:0;}
    50%{opacity:1;}
    100%{opacity:1;}
}
</style>
