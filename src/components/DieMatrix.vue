<template>
	<div v-if="showDie" :class="['die', 'matrix']" :style="[dieDims]">
		<div :class="['face', 'front', faces.f_front, {doubleLetter: faces.f_front.length >= 2}]" :style="[dieDims, frontFaceRotate]">{{ faces.f_front }}</div>
		<div v-if="showAllFaces" :class="['face', 'back', {doubleLetter: faces.f_back.length >= 2}]" :style="[dieDims]">{{ faces.f_back }}</div>
		<div v-if="showAllFaces" :class="['face', 'top', {doubleLetter: faces.f_top.length >= 2}]" :style="[dieDims]">{{ faces.f_top }}</div>
		<div v-if="showAllFaces" :class="['face', 'bottom', {doubleLetter: faces.f_bottom.length >= 2}]" :style="[dieDims]">{{ faces.f_bottom }}</div>
		<div v-if="showAllFaces" :class="['face', 'left', {doubleLetter: faces.f_left.length >= 2}]" :style="[dieDims]">{{ faces.f_left }}</div>
		<div v-if="showAllFaces" :class="['face', 'right', {doubleLetter: faces.f_right.length >= 2}]" :style="[dieDims]">{{ faces.f_right }}</div>
	</div>
</template>
<script>
	export default {
		name: 'Die',
		data() {
			const half = (this.dieSize / 2);
			return {
				showDie: false,
				showAllFaces: true,
				matrixChars: "ऄअआइईउऊऋऌऍऎएऐऑऒओऔकखगघङचछजझञटठडढणतथदधनऩपफबभमयरऱलळऴवशषसह१२३४५६७८९॰ॱॲॳॴॵॶॷॸॹॺॻॼॽॾॿঀঠডঢণতথদধনপফবভমযরলশষসহঽଋଌଏଐଓଔକଖଗଘଙଚଛଜଝଞଟଠଡଢଣତଥଦଧନପଫବଭମଯରଲଳଵଶଷସହ଼ଽାିୀୁୂୃୄେୈୋୌ୍୕ୖୗଡ଼ଢ଼ୟୠୡ୦୧୨୩୪୫୬୭୮୯୰ୱ୲୳୴୵୶୷ஃஅஆஇஈஉஊஎஏஐஒஓಆಇಈಉಊಋಌಎಏಐಒಓಔಕಖಗಘಙಚಛಜಝಞಟಠಡಢಣತಥದഖഗഘങചഛജഝഞടഠഡഢണതഥദധനഩപഫബഭമയരറലളഴവശഷസഹทธนบปผฝพฟภมยรฤลฦวศษสหฬགྷངཅཆཇཉཊཋཌཌྷཎཏཐདདྷནཔཕབབྷམཙཚཛཛྷཝཞཟའཡརལཤཥསཧბგდევზთიკლმნოპჟრსტუᆁᆂᆃᆄᆅᆆᆇᆈᆉᆊᆋᆌᆍᆎᆏᆐᆑᆒᆓᆔᆕᆖᆗᆘᆙᆚᆛᆜᆟᆠᆡᆣᆤᆥᆦᆧሐሑሒሓሔሕሖሗመሙሚማሜምሞሟሠሡሢሣሤሥሦሧረሩሪራሬርሮሯሰሱሲሳሴስሶሷሸሹሺሻሼሽሾሿቀቁቂቃቄቅቆቇቈቊቋቌቍቐቑቒቓቔቕឈញដឋឌឍណតថទធនបផពភមយរលぁあぃいぅうぇえぉおかがきぎくぐけげこごさざしじすずせぜそぞただちぢっつづてでとどなにぬねのはばぱひびぴふぶぷへべぺほぼぽま",
				faces: {
					f_back: '',
					f_bottom: '',
					f_front: '',
					f_left: '',
					f_right: '',
					f_top: ''
				},
				dieDims: {
					height: this.dieSize + "px",
					width: this.dieSize + "px",
					fontSize: (this.dieSize * 0.8) + "px",
				},
				frontFaceRotate: {
					transform: "rotate("+(Math.floor(Math.random() * 4) * 90)+"deg)"
				},
				animationDelay: 0,
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
			},
			getRandomMatrixChar() {
				return this.matrixChars.charAt(Math.floor(Math.random() * this.matrixChars.length));
			},
		},
		watch: {
			isGameRunning() {
				if (this.isGameRunning) {
					this.showDie = false;
					this.faces.f_front = "";
					this.faces.f_back = "";
					this.faces.f_bottom = "";
					this.faces.f_left = "";
					this.faces.f_right = "";
					this.faces.f_top = "";
					this.animationDelay = Math.floor(Math.random() * 3000);

					if (this.template) {
						const pos = Math.floor(Math.random() * this.template.length);
						const templateArray = this.template.split('');
						this.faces.f_front = templateArray[pos].replace(/@/, 'An')
															.replace(/\$/, 'Er')
															.replace(/&/, 'He')
															.replace(/#/, 'In')
															.replace(/%/, 'Qu')
															.replace(/\*/, 'Th');
					}

					setTimeout(() => {
						this.faces.f_back = this.getRandomMatrixChar();
						this.faces.f_bottom = this.getRandomMatrixChar();
						this.faces.f_left = this.getRandomMatrixChar();
						this.faces.f_right = this.getRandomMatrixChar();
						this.faces.f_top = this.getRandomMatrixChar();
						this.rollTheDice();
					}, 200);

					setTimeout(() => {
						this.showDie = true;
					}, this.animationDelay);

					setTimeout(() => {
						this.showAllFaces = false;
					}, this.animationDelay+3000);

				}
			},
			dieSize() {
				const half = this.dieSize / 2;
				this.dieDims = {
					height: this.dieSize + "px",
					width: this.dieSize + "px",
					fontSize: (this.dieSize * 0.8) + "px"
				}
			}
		}
	}
</script>
