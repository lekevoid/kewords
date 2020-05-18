<template>
	<!-- <div :class="['die', { no_anim: !isGameRunning }]" :style="initialStyles"> -->
	<div :class="['die', animClass]" :style="[initialStyles, dieDims]">
		<div :class="['face', 'front', faces.f_front, {doubleLetter: faces.f_front.length >= 2}]" :style="[facesTransforms.f_front, dieDims]">{{ faces.f_front }}</div>
		<div v-if="showAllFaces" :class="['face', 'back', {doubleLetter: faces.f_back.length >= 2}]" :style="[facesTransforms.f_back, dieDims]">{{ faces.f_back }}</div>
		<div v-if="showAllFaces" :class="['face', 'top', {doubleLetter: faces.f_top.length >= 2}]" :style="[facesTransforms.f_top, dieDims]">{{ faces.f_top }}</div>
		<div v-if="showAllFaces" :class="['face', 'bottom', {doubleLetter: faces.f_bottom.length >= 2}]" :style="[facesTransforms.f_bottom, dieDims]">{{ faces.f_bottom }}</div>
		<div v-if="showAllFaces" :class="['face', 'left', {doubleLetter: faces.f_left.length >= 2}]" :style="[facesTransforms.f_left, dieDims]">{{ faces.f_left }}</div>
		<div v-if="showAllFaces" :class="['face', 'right', {doubleLetter: faces.f_right.length >= 2}]" :style="[facesTransforms.f_right, dieDims]">{{ faces.f_right }}</div>
	</div>
</template>
<script>
	export default {
		name: 'Die',
		data() {
			const half = (this.dieSize / 2);
			return {
				animClass: "static",
				showAllFaces: true,
				faces: {
					f_back: '',
					f_bottom: '',
					f_front: '',
					f_left: '',
					f_right: '',
					f_top: ''
				},
				initialStyles: {
					transform: "rotate3d(0, 0, 0, 0deg)",
					transition: "none"
				},
				dieDims: {
					height: this.dieSize + "px",
					width: this.dieSize + "px",
					fontSize: (this.dieSize * 0.8) + "px"
				},
				facesTransforms: {
					// Necessary evil...
					f_back: {
						transform: "rotateY(-180deg) translateZ("+half+"px)"
					},
					f_bottom: {
						transform: "rotateX(90deg) translateZ("+half+"px)"
					},
					f_front: {
						transform: "rotateY(0deg) translateZ("+half+"px)"
					},
					f_left: {
						transform: "rotateY(90deg) translateZ("+half+"px)"
					},
					f_right: {
						transform: "rotateY(-90deg) translateZ("+half+"px)"
					},
					f_top: {
						transform: "rotateX(-90deg) translateZ("+half+"px)"
					}
				}
			}
		},
		props: ['dieSize', 'template', 'isGameRunning'],
		created() {},
		methods: {
			plusOrMinusOne() {
				let num = Math.round(Math.random());
				if (num === 0) { num = -1; }
				return num;
			},
			rollTheDice() {
				this.showAllFaces = true;
				this.animClass = `anim_${Math.ceil(Math.random() * 7)}_${Math.ceil(Math.random() * 4)}`;
			}
		},
		watch: {
			isGameRunning() {
				if (this.isGameRunning) {
					this.faces.f_front = "";
					this.faces.f_back = "";
					this.faces.f_bottom = "";
					this.faces.f_left = "";
					this.faces.f_right = "";
					this.faces.f_top = "";

					if (this.template) {
						var pos = Math.floor(Math.random() * this.template.length);
						var templateArray = this.template.split('');
						templateArray.forEach((t, index) => {
							templateArray[index] = templateArray[index].replace(/@/, 'An');
							templateArray[index] = templateArray[index].replace(/\$/, 'Er');
							templateArray[index] = templateArray[index].replace(/&/, 'He');
							templateArray[index] = templateArray[index].replace(/#/, 'In');
							templateArray[index] = templateArray[index].replace(/%/, 'Qu');
							templateArray[index] = templateArray[index].replace(/\*/, 'Th');
						});
					}

					setTimeout(() => {
						this.rollTheDice();

						if (this.template) {
							this.faces.f_front = templateArray.splice(pos, 1)[0];
							this.faces.f_back = templateArray[0];
							this.faces.f_bottom = templateArray[1];
							this.faces.f_left = templateArray[2];
							this.faces.f_right = templateArray[3];
							this.faces.f_top = templateArray[4];
						}
					}, 600);

					setTimeout(() => {
						this.showAllFaces = false;
					}, 3000);

				}
			},
			dieSize() {
				const half = this.dieSize / 2;
				this.dieDims = {
					height: this.dieSize + "px",
					width: this.dieSize + "px",
					fontSize: (this.dieSize * 0.8) + "px"
				}
				this.facesTransforms = {
					// Necessary evil...
					f_back: {
						transform: "rotateY(-180deg) translateZ("+half+"px)"
					},
					f_bottom: {
						transform: "rotateX(90deg) translateZ("+half+"px)"
					},
					f_front: {
						transform: "rotateY(0deg) translateZ("+half+"px)"
					},
					f_left: {
						transform: "rotateY(90deg) translateZ("+half+"px)"
					},
					f_right: {
						transform: "rotateY(-90deg) translateZ("+half+"px)"
					},
					f_top: {
						transform: "rotateX(-90deg) translateZ("+half+"px)"
					}
				}
			}
		}
	}
</script>
