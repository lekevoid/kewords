<template>
	<div id="grid" :class="gridClass">
		<div v-for="(y, indexA) in gridY" :key="indexA+y" class="row" :id="y">
			<div v-for="(x, indexB) in gridX" :key="indexB+x" class="cell" :style="cellSize">
				<Die :template="getDie(x,y)" :isGameRunning="isGameRunning" :dieSize="dieSize" />
			</div>
		</div>
	</div>
</template>
<script>
	import Die from "./Die.vue";

	export default {
		name: "Grid",
		components: {
			Die
		},
		data() {
			return {
				diceArray: this.allDiceTemplates,
				emptyDieTemplate: "??????",
				cellSize: {
					height: "100px",
					width: "100px"
				},
				dieSize: 80
			}
		},
		props: ["allDiceTemplates", "gridX", "gridY", "isGameRunning", "forceLetterResize"],
		computed: {
			biggestSide() {
				return Math.max(this.gridX, this.gridY);
			},
			gridClass() {
				return "grid"+this.biggestSide;
			}
		},
		methods: {
			getDie(x, y) {
				if (this.isGameRunning) {
					// Formula that calculates the position of the
					// die's parent cell in the grid. This is my
					// Philosopher's stone and I'm KEEPING IT !!!
					return this.diceArray[x - 1 + ((y - 1) * this.gridX)];
				} else {
					return this.emptyDieTemplate;
				}
			},
			resizeCells() {
				let biggestViewDim = Math.min(document.getElementById("right").offsetHeight, document.getElementById("right").offsetWidth) * 0.8;
				this.cellSize = {
					height: Math.max(50, Math.floor(biggestViewDim / this.biggestSide)) + "px",
					width: Math.max(50, Math.floor(biggestViewDim / this.biggestSide)) + "px"
				}
				this.dieSize = Math.max(40, Math.floor(biggestViewDim / this.biggestSide)) * 0.8;
			}
		},
		created() {
			setTimeout(this.resizeCells, 100);
			window.addEventListener("resize", this.resizeCells);
		},
		watch: {
			gridX() {
				this.resizeCells();
			},
			gridY() {
				this.resizeCells();
			}
		}
	}
</script>
