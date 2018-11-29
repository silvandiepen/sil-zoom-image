<template>
	<div
		ref="image"
		:class="{'active' : isActive}"
		:style="{ backgroundImage: `url('${src}')`, backgroundSize: size }"
		class="zoom-image"
		@click="isActive = !isActive"
		@mouseout="isActive = false"
		@mousemove="moveMouse"
	>
		<div
			v-if="showOverlay"
			class="zoom-image__overlay"
		></div>
		<div
			v-if="showIcon"
			class="zoom-image__icon"
		></div>
	</div>
</template>

<script>
export default {
	props: {
		src: {
			type: String,
			default: ''
		},
		size: {
			type: String,
			default: 'cover'
		},
		showOverlay: {
			type: Boolean,
			default: false
		},
		showIcon: {
			type: Boolean,
			default: false
		},
		cursor: {
			type: Array,
			default: () => []
		}
	},
	data() {
		return {
			isActive: false,
			moved: {
				x: 0,
				y: 0
			}
		};
	},
	watch: {
		isActive: function() {
			this.setCursor();
		}
	},
	mounted() {
		this.setCursor();
	},
	methods: {
		setCursor() {
			if(!process.browser) return; 
			if (this.$props.cursor.length == 2) {
				if (this.isActive) {
					this.$refs.image.style.setProperty('--zoom-image-cursor-inactive', `url('${this.$props.cursor[1]})'`);
				} else {
					this.$refs.image.style.setProperty('--zoom-image-cursor-active', `url('${this.$props.cursor[0]})'`);
				}
			} else if (this.$props.cursor.length == 1) {
				this.$refs.image.style.setProperty('--zoom-image-cursor-active', `url('${this.$props.cursor[0]})'`);
			}
			
		},
		moveMouse(e) {
			if(!process.browser) return; 
			if (this.isActive) {
				let size = this.$refs.image.getBoundingClientRect();
				let position = {
					x: (e.offsetX / size.width) * 100,
					y: (e.offsetY / size.height) * 100
				};
				this.moved.x = e.offsetX;
				this.moved.y = e.offsetY;

				this.$refs.image.style.setProperty('--x', position.x + '%');
				this.$refs.image.style.setProperty('--y', position.y + '%');
			}
		}
	}
};
</script>

<style>
.zoom-image {
	--x: 0px;
	--y: 0px;
	width: 100%;
	min-height: 100%;
	height: 0;
	box-shadow: 0 0 2rem 0 rgba(0, 0, 0, 0);
	background-position: center center;
	background-repeat: no-repeat;
	position: relative;
	overflow: hidden;
	transition: background-size 0.3s ease-in-out;
	z-index: 1000;
	mix-blend-mode: multiply;
}
.zoom-image__overlay {
	display: block;
	width: 100%;
	height: 100%;
	position: absolute;
	background-color: rgba(0, 0, 0, 0.25);
	opacity: 0;
	transition: opacity 0.3s ease-in-out;
}
.zoom-image__icon {
	position: absolute;
	top: 50%;
	left: 50%;
	width: 2.75rem;
	height: 2rem;
	transform: translate(-50%, 100vw) rotate(0deg);
	color: white;
	transition: transform 0.3s ease-in-out, opacity 0.3s ease-in-out;
	background-image: radial-gradient(transparent 50%, white 50%, white calc(50% + 3px), transparent calc(50% + 3px)),
		linear-gradient(to bottom, white, white);
	background-size: 2rem 2rem, 1rem 3px;
	background-repeat: no-repeat, no-repeat;
	background-position: center left, center right;
	cursor: var(--zoom-image-cursor-inactive), auto;
}
.zoom-image:hover .zoom-image__overlay {
	opacity: 1;
}
.zoom-image:hover .zoom-image__icon {
	transform: translate(-50%, -50%) rotate(45deg);
}

.zoom-image.active {
	background-size: auto !important;
	background-position: var(--x) var(--y);
	cursor: var(--zoom-image-cursor-active), auto;
}
.zoom-image.active .zoom-image__overlay,
.zoom-image.active .zoom-image__icon {
	opacity: 0;
}
</style>
