<template>
	<div class="zoom-image" :class="{'active' : active}" ref="image" :style="{ backgroundImage: `url('${src}')`, backgroundSize: size }" @click="active = !active" @mouseout="active = false" @mousemove="moveImage" @touchmove="movedTouch"></div>
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
		}
	},
	data() {
		return {
			active: false,
			moved: {
				x: 0,
				y: 0
			}
		};
	},
	methods: {
		moveImage(e) {
			let size = this.$refs.image.getBoundingClientRect();
			let position = {
				x: (e.offsetX / size.width) * 100,
				y: (e.offsetY / size.height) * 100
			};
			this.moved.x = e.offsetX;
			this.moved.y = e.offsetY;

			this.$refs.image.style.setProperty('--x', position.x + '%');
			this.$refs.image.style.setProperty('--y', position.y + '%');
		},
		movedTouch(e) {
			this.moved = {
				x: e.offsetX,
				y: e.offsetY
			};
		}
	}
};
</script>

<style scoped>
.zoom-image {
	--x: 0px;
	--y: 0px;
	width: 100%;
	height: 0;
	padding-bottom: 56%;
	border: 2px solid white;
	box-shadow: 0 0 2rem 0 rgba(0, 0, 0, 0);
	background-position: center center;
	background-repeat: no-repeat;
	position: relative;
	overflow: hidden;
	transition: box-shadow 0.3s ease-in-out, background-size 0.3s ease-in-out;
	background-color: white;
}
.zoom-image:before {
	content: '';
	display: block;
	width: 100%;
	height: 100%;
	position: absolute;
	background-color: rgba(0, 0, 0, 0.25);
	opacity: 0;
	transition: opacity 0.3s ease-in-out;
}
.zoom-image:after {
	content: '';
	position: absolute;
	top: 50%;
	left: 50%;
	width: 2.75rem;
	height: 2rem;
	transform: translate(-50%, 100vw) rotate(0deg);
	color: white;
	transition: transform 0.3s ease-in-out;
	background-image: radial-gradient(
			transparent 50%,
			white 50%,
			white calc(50% + 3px),
			transparent calc(50% + 3px)
		),
		linear-gradient(to bottom, white, white);
	background-size: 2rem 2rem, 1rem 3px;
	background-repeat: no-repeat, no-repeat;
	background-position: center left, center right;
}
.zoom-image:hover .zoom:before {
	opacity: 1;
}
.zoom-image:hover .zoom:after {
	transform: translate(-50%, -50%) rotate(45deg);
}

.zoom-image.active {
	background-size: auto !important;
	background-position: var(--x) var(--y);
	box-shadow: 0 0 2rem 0 rgba(0, 0, 0, 0.25);
}
.zoom-image.active :before,
.zoom-image.active :after {
	opacity: 0;
}
</style>