<template>
	<div id="dictionary" @submit.prevent="apiCall()" :class="[{ open : open, valid : valid === true, invalid : valid === false }]">
		<button @click="toggleOpen()" class="btn_open">
			<img :src="icon_dictionary" />
		</button>
		<form>
			<input type="text" v-model="word" class="input_word" id="input_word" autocorrect="off" autocapitalize="none" />
			<button type="submit" @click.prevent="apiCall()" class="btn_search">
				<img :src="icon_search" />
			</button>
		</form>
		<div class="definitions" v-if="valid">
			<div class="def" v-for="(d, i) in definitions" :key="i">
				<div class="row">
					<div class="word">{{d.word}} :</div>
					<div class="type">{{d.type}}</div>
				</div>
				<div class="shortdef" v-for="(s, k) in d.defs" :key="k">
					{{s}}
				</div>
			</div>
		</div>
		<div class="definitions not_found" v-if="valid === false">Word not found.</div>
	</div>
</template>
<script>
	import icon_dictionary from "../assets/img/icon_dictionary.svg";
	import icon_search from "../assets/img/icon_search.svg";

	const axios = require("axios");

	export default {
		name: "Dico",
		data() {
			return {
				word: "",
				loading: false,
				valid: null,
				apiResult: "",
				apiError: "",
				definitions: "",
				icon_dictionary: icon_dictionary,
				icon_search: icon_search,
			}
		},
		props: {
			open: Boolean
		},
		methods: {
			resetState() {
				this.loading = false;
				this.valid = null;
				this.apiResult = "";
				this.apiError = "";
				document.getElementById("input_word").blur();
			},
			apiCall() {
				this.resetState();
				this.loading = true;

				this.apiError = this.apiResult = "";
				this.loading = true;

				axios.get("https://dictionaryapi.com/api/v3/references/collegiate/json/" + this.word + "?key=426196c0-614a-4f71-8ae1-e085f3008e8a")
					.then((response) => {
						const { data } = response;

						if (data.length > 0) {
							this.valid = false;
							for (let x=0; x<data.length; x++) {
								if (data[x] && data[x].meta && data[x].meta.id.includes(this.word.toLowerCase())) {
									this.valid = true;
									this.formatDefinition(data);
								}

								if (data[x] && data[x].meta && data[x].meta.stems && data[x].meta.stems.includes(this.word.toLowerCase())) {
									this.valid = true;
									this.formatDefinition(data);
								}
							}
						} else {
							this.valid = false;
							this.formatDefinition(null);
						}

						this.loading = false;
					})
					.catch((error) => {
						this.error = error;
						console.log(this.error);
						this.loading = false;
					})
					.finally(() => {
						// always executed
					});
			},
			formatDefinition(data) {
				if (data) {
					this.definitions = data.map((d, i) => {
						return {
							word: this.word,
							type: d.fl,
							defs: this.capitalize(d.shortdef)
						}
					});
				}
			},
			capitalize(str) {
				let r = [];
				if (typeof(str) === "object") {
					r = str.map((s) => {
						return s.slice(0, 1).toUpperCase() + s.slice(1)
					})
				}
				console.log(r);
				return r;
			},
			toggleOpen() {
				this.$parent.dictionaryOpen = !this.$parent.dictionaryOpen;
			}
		},
		created() {

		},
		watch: {

		}
	}
</script>
