<template>
	<div id="app" :class="[{ is_running: gameRunning, is_paused: gamePaused, is_ended: gameEnded, dictionary_open: dictionaryOpen }]">
		<div class="wrap" id="left">
			<ParamsForm :startGridX="gridX" :startGridY="gridY" :isGameRunning="gameRunning" :lang="language" />
			<Timer :initialTime="initialTimeSeconds" :isGameRunning="gameRunning" :isGamePaused="gamePaused" :isGameEnded="gameEnded" />
			<div id="actions">
				<div :class="['plays', { visible: !gameRunning }]">
					<button @click="toggleGame" class="play">
						<font-awesome-icon icon="play" />
					</button>
				</div>
				<div :class="['stops', { visible: gameRunning }]">
					<button @click="toggleGame" class="stop">
						<font-awesome-icon icon="stop" />
					</button>
					<button @click="togglePause" class="pause">
						<font-awesome-icon icon="pause" />
					</button>
				</div>
			</div>
		</div>
		<div class="wrap" id="right">
			<Grid :gridX="gridX" :gridY="gridY" :allDiceTemplates="dice" :isGameRunning="gameRunning" />
		</div>
		<Dico :open="dictionaryOpen" />
		<div id="pauseOverlay" v-if="gamePaused" @click="togglePause">
			<font-awesome-icon icon="pause" />
		</div>
	</div>
</template>
<script>
	import Grid from "./components/Grid.vue";
	import ParamsForm from "./components/ParamsForm.vue";
	import Timer from "./components/Timer.vue";
	import Dico from "./components/Dico.vue";

	import { library } from "@fortawesome/fontawesome-svg-core";
	import { faPlay } from "@fortawesome/free-solid-svg-icons";
	import { faStop } from "@fortawesome/free-solid-svg-icons";
	import { faPause } from "@fortawesome/free-solid-svg-icons";
	library.add(faPlay);
	library.add(faStop);
	library.add(faPause);

	import "./assets/styles.css";

	export default {
		/* eslint-disable no-console */
		name: "app",
		components: { Grid, ParamsForm, Timer, Dico },
		data() {
			return {
				diceTemplates: ["aaafrs", "aaeeee", "aaeeoo", "aafirs", "abdeio", "adennn", "aeeeem", "aeegmu", "aegmnn", "aeilmn", "aeinou", "afirsy", "@$&#%*", "bbjkxz", "ccenst", "cddlnn", "ceiitt", "ceipst", "cfgnuy", "ddhnot", "dhhlor", "dhhnow", "dhlnor", "ehilrs", "eiilst", "eilpst", "eioeio", "emttto", "ensssu", "gorrvw", "hirstv", "hoprst", "iprsyy", "jk%wxz", "nootuw", "ooottu"],
				gameRunning: false,
				gamePaused: false,
				gameEnded: false,
				gridX: 6,
				gridY: 6,
				dice: [],
				language: "en",
				initialTimeSeconds: 300,
				forceLetterResize: true,
				dictionaryOpen: false
			}
		},
		methods: {
			resizeGrid(x, y) {
				this.gridX = parseInt(x);
				this.gridY = parseInt(y);
			},
			addDiceUntilGridIsFull() {
				let gridSize = this.gridX * this.gridY;
				if (this.diceTemplates.length < gridSize) {
					for (let a = this.diceTemplates.length; a <= gridSize; a++) {
						let pos = Math.floor(Math.random() * this.diceTemplates.length);
						this.dice.push(this.diceTemplates[pos]);
					}
				}
			},
			shuffleDice(array) {
				var currentIndex = array.length,
					temporaryValue, randomIndex;
				// While there remain elements to shuffle...
				while (0 !== currentIndex) {
					// Pick a remaining element...
					randomIndex = Math.floor(Math.random() * currentIndex);
					currentIndex -= 1;
					// And swap it with the current element.
					temporaryValue = array[currentIndex];
					array[currentIndex] = array[randomIndex];
					array[randomIndex] = temporaryValue;
				}
				this.dice = array;
			},
			changeLang(to) {
				this.language = to;
				switch (this.language) {
					case "fr":
						this.diceTemplates = ["lenuyg", "elupst", "zdvnea", "sdtnoe", "amoris", "fxraoi", "mo%abj", "fsheei", "hrsnei", "etnkou", "tarilb", "tieaoa", "acepdm", "rlasec", "uliwer", "vgtnie"];
						break;
					default:
						this.diceTemplates = ["aaafrs", "aaeeee", "aaeeoo", "aafirs", "abdeio", "adennn", "aeeeem", "aeegmu", "aegmnn", "aeilmn", "aeinou", "afirsy", "@$&#%*", "bbjkxz", "ccenst", "cddlnn", "ceiitt", "ceipst", "cfgnuy", "ddhnot", "dhhlor", "dhhnow", "dhlnor", "ehilrs", "eiilst", "eilpst", "eioeio", "emttto", "ensssu", "gorrvw", "hirstv", "hoprst", "iprsyy", "jk%wxz", "nootuw", "ooottu"];
				}
			},
			toggleGame() {
				/* Rest Game State */
				this.gameEnded = false;
				this.gameRunning = !this.gameRunning;
				/* Generate Dice */
				if (this.gameRunning) {
					this.diceTemplates.forEach((t) => {
						this.dice.push(t);
					});
					this.addDiceUntilGridIsFull();
					this.shuffleDice(this.dice);
				}
			},
			togglePause() {
				this.gamePaused = !this.gamePaused;
			},
			setStartingTimer(seconds) {
				this.initialTimeSeconds = seconds;
			},
			endGame() {
				this.gameEnded = true;
			}
		}
	}
</script>
