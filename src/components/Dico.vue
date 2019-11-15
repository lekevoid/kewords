<template>
	<form id="dictionary" @submit.prevent="apiCall()" :class="[{ valid : valid === true, invalid : valid === false }]">
		<input type="text" v-model="word" />
		<button type="submit" @click.prevent="apiCall()">Search</button>
	</form>
</template>
<script>
	import { library } from "@fortawesome/fontawesome-svg-core";
	import { faLink } from "@fortawesome/free-solid-svg-icons";
	library.add(faLink);

	const axios = require("axios");

	export default {
		name: "Dico",
		data() {
			return {
				word: "",
				loading: false,
				valid: null,
				apiResult: "",
				apiError: ""
			}
		},
		props: [],
		methods: {
			resetState() {
				this.loading = false;
				this.valid = null;
				this.apiResult = "";
				this.apiError = "";
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
								}

								if (data[x] && data[x].meta && data[x].meta.stems && data[x].meta.stems.includes(this.word.toLowerCase())) {
									this.valid = true;
								}
							}
						} else {
							this.valid = false;
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
			}
		},
		created() {

		},
		watch: {

		}
	}
</script>
