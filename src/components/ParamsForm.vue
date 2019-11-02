<template>
	<div id="params">
		<form>
			<div class="row" id="langs">
				<div class="field">
					<button type="button" id="lang_en" @click="changeLang('en')" :class="{active: lang === 'en'}">EN</button>
					<button type="button" id="lang_fr" @click="changeLang('fr')" :class="{active: lang === 'fr'}">FR</button>
				</div>
			</div>
			<div id="core_params">
				<h1>Grid Dimensions</h1>
				<div :class="['row', { 'lock_x_y' : lockXY }]" id="dimensions">
					<div class="field axis_x">
						<input type="number" readonly v-model="gridX" min="3" max="15" :disabled="isGameRunning" />
						<button type="button" class="num_up" @click="changeX(1)" :disabled="isGameRunning">+</button>
						<button type="button" class="num_down" @click="changeX(-1)" :disabled="isGameRunning">&ndash;</button>
					</div>
					<span>x</span>
					<div class="field axis_y">
						<input type="number" readonly v-model="gridY" min="3" :disabled="isGameRunning || lockXY" />
						<button type="button" class="num_up" @click="changeY(1)" :disabled="isGameRunning || lockXY">+</button>
						<button type="button" class="num_down" @click="changeY(-1)" :disabled="isGameRunning || lockXY">&ndash;</button>
					</div>
					<button type="button" id="lock_x_y" :class="{ active: lockXY }" :disabled="isGameRunning" @click="toggleLockXY">
						<font-awesome-icon icon="unlink" v-if="!lockXY" />
						<font-awesome-icon icon="link" v-if="lockXY" />
					</button>
				</div>
				<h1>Time</h1>
				<div class="row" id="time">
					<div class="field">
						<input type="number" readonly v-model="minutes" min="1" max="99" :disabled="isGameRunning" />
						<button type="button" class="num_up" @click="changeMinutes(1)" :disabled="isGameRunning">+</button>
						<button type="button" class="num_down" @click="changeMinutes(-1)" :disabled="isGameRunning">&ndash;</button>
					</div>
					<div id="word_minutes">Mins</div>
				</div>
			</div>
		</form>
	</div>
</template>
<script>
	import {
		library
	} from "@fortawesome/fontawesome-svg-core"
	import {
		faLink
	} from "@fortawesome/free-solid-svg-icons"
	import {
		faUnlink
	} from "@fortawesome/free-solid-svg-icons"
	library.add(faLink)
	library.add(faUnlink)
	export default {
		name: "ParamsForm",
		data() {
			return {
				gridX: 0,
				gridY: 0,
				lockXY: true,
				minutes: 5
			}
		},
		props: ["lang", "startGridX", "startGridY", "isGameRunning"],
		methods: {
			grid() {
				if (this.lockXY) {
					this.$parent.resizeGrid(this.gridX, this.gridX);
				} else {
					this.$parent.resizeGrid(this.gridX, this.gridY);
				}
			},
			changeX(by) {
				this.gridX += by;
				if (this.gridX < 3) {
					this.gridX = 3;
				}
				if (this.gridX > 15) {
					this.gridX = 15;
				}
			},
			changeY(by) {
				this.gridY += by;
				if (this.gridY < 3) {
					this.gridY = 3;
				}
				if (this.gridY > 15) {
					this.gridY = 15;
				}
			},
			toggleLockXY() {
				this.lockXY = !this.lockXY;
			},
			changeMinutes(by) {
				this.minutes += by;
				if (this.minutes < 1) {
					this.minutes = 1;
				}
				if (this.minutes > 30) {
					this.minutes = 30;
				}
			},
			changeLang(to) {
				this.$parent.changeLang(to);
			}
		},
		created() {
			this.gridX = this.startGridX;
			this.gridY = this.startGridY;
		},
		watch: {
			gridX() {
				this.grid();
				if (this.lockXY) {
					this.gridY = this.gridX;
				}
			},
			gridY() {
				this.grid();
			},
			lockXY() {
				this.grid();
			},
			minutes() {
				this.$parent.setStartingTimer((this.minutes * 60));
			}
		}
	}
</script>