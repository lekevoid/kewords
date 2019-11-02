<template>
	<div id="timer">
		<div id="view_options">
			<button type="button" @click="setTimerView('timer')" :disabled="view === 'timer'">
				<font-awesome-icon icon="stopwatch" /></button>
			<!-- <button type="button" @click="setTimerView('hourglass')" :disabled="view === 'hourglass'">
				<font-awesome-icon icon="hourglass-half" /></button> -->
			<button type="button" @click="setTimerView('none')" :disabled="view === 'none'">
				<font-awesome-icon icon="eye-slash" /></button>
		</div>
		<div id="view_timer" :class="{ 'blink' : isGameEnded, 'visible' : view === 'timer' }">
			<div id="time">
				<span id="minutes" class="number">{{ timer.minutes }}</span>
				<span class="colon">:</span>
				<span id="seconds" class="number">{{ timer.seconds }}</span>
			</div>
		</div>
		<!-- <div id="view_hourglass" :class="{ blink: isGameEnded }" v-if="(view === 'hourglass')">
			<div class="socle">&nbsp;</div>
			<div class="hide right">&nbsp;</div>
			<div class="hide left">&nbsp;</div>
			<div class="container" id="hourglass_top">
				<div class="reflection">&nbsp;</div>
				<div class="sand" id="top_sand" :style="remainingSand"> &nbsp; </div>
			</div>
			<div class="container" id="hourglass_bottom">
				<div id="grain" :class="{is_running: (isGameRunning && !isGamePaused)}">&nbsp;</div>
				<div class="reflection">&nbsp;</div>
				<div class="sand" id="bottom_sand" :style="droppedSand"> &nbsp; </div>
			</div>
			<div class="socle">&nbsp;</div>
		</div> -->
	</div>
</template>
<script>
	import {
		library
	} from '@fortawesome/fontawesome-svg-core'
	import {
		faHourglassHalf
	} from '@fortawesome/free-solid-svg-icons'
	import {
		faStopwatch
	} from '@fortawesome/free-solid-svg-icons'
	import {
		faEyeSlash
	} from '@fortawesome/free-solid-svg-icons'
	library.add(faHourglassHalf)
	library.add(faStopwatch)
	library.add(faEyeSlash)
	export default {
		name: 'Timer',
		data() {
			return {
				remainingTime: this.initialTime,
				remainingTimePercent: 100,
				timer: {
					minutes: "0",
					seconds: "00"
				},
				remainingSand: {
					height: "80%"
				},
				droppedSand: {
					height: "0%"
				},
				view: 'timer'
			}
		},
		props: ['initialTime', 'isGameRunning', 'isGamePaused', 'isGameEnded'],
		created() {},
		methods: {
			clock() {
				if (this.remainingTime > 0) {
					this.remainingTime -= 0.1;
					this.remainingTimePercent = this.remainingTime / this.initialTime * 100;
					if (this.isGameRunning && !this.isGamePaused) {
						setTimeout(this.clock, 100);
					}
				} else {
					this.remainingTime = 0;
					this.remainingTimePercent = 0;
					this.$parent.endGame();
				}
			},
			setTimerView(v) {
				this.view = v;
			}
		},
		watch: {
			initialTime() {
				this.remainingTime = this.initialTime;
			},
			isGameRunning() {
				this.remainingTime = this.initialTime;
				if (this.isGameRunning && !this.isGamePaused) {
					setTimeout(this.clock, 2000);
				}
			},
			isGamePaused() {
				if (this.isGameRunning && !this.isGamePaused) {
					setTimeout(this.clock, 1000);
				}
			},
			remainingTimePercent() {
				if (this.remainingTime < 0) {
					this.remainingTime = 0;
				}
				/* Timer */
				this.timer.seconds = Math.ceil(this.remainingTime % 60);
				this.timer.minutes = Math.floor(this.remainingTime / 60);
				if (this.timer.seconds >= 60) {
					this.timer.seconds -= 60;
					this.timer.minutes += 1;
				}
				if (this.timer.seconds < 10) {
					this.timer.seconds = "0" + this.timer.seconds;
				}
				/* Hourglass */
				this.remainingSand.height = (this.remainingTimePercent * 0.8) + "%";
				this.droppedSand.height = (80 - (this.remainingTimePercent * 0.8)) + "%";
			}
		}
	}
</script>