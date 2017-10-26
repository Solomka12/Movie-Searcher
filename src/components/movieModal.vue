<template>
	<div class="movie-modal">
		<div class="md-modal" :class="{ 'md-modal-show': show }">
			<div class="md-content row justify-content-start" :style="{ backgroundImage: backImage ? 'linear-gradient(to top,rgba(0,0,0,.9) 0,rgba(0,0,0,.9) 0), url(https://image.tmdb.org/t/p/w1000' + backImage + ')' : 'none' }">
				<slot></slot>
				<div class="md-close md-content__close" @click="closeModal()"><b>&#10006;</b></div>
			</div>
		</div>

		<div class="md-overlay" @click="closeModal()"></div>
	</div>
</template>

<script>
export default {
	props: ['show', 'backImage'],
	data () {
		return {

		}
	},
	methods: {
		closeModal: function() {
			this.$emit('modalClosed', false)
		},
		getBG: function() {

		}
	}
}
</script>

<style scoped>
.md-modal {
	position: fixed;
	top: 50%;
	left: 50%;
	width: 95%;
	max-width: 1000px;
	height: auto;
	max-height: 100%;
	z-index: 10000;
	visibility: hidden;
	overflow-x: hidden;
	transform: translateX(-50%) translateY(-50%);

}
.md-modal-show {
	visibility: visible;
}
.md-content {
	background-position: center center;
	background-size: cover;
	background-repeat: no-repeat;
	position: relative;
	border-radius: 5px;
	padding: 30px 20px 10px;
	transform: scale(0.7);
	opacity: 0;
	transition: all 0.3s;
	margin: 0;
}
.md-content:after {
	content: '';
	display: block;
	height: 0;
	clear: both;
	visibility: hidden;
}
.md-modal-show .md-content {
	transform: scale(1);
	opacity: 1;
}
.md-content__close {
	top: 5px;
	position: absolute;
	right: 15px;
	font-size: 1.6em;
	color: #e76;
	cursor: pointer;
}
.md-content__close:hover {
	color: #b23;
}
.md-overlay {	
	position: fixed;
	width: 100%;
	height: 100%;
	top: 0;
	left: 0;
	z-index: 9999;
	opacity: 0;
	visibility: hidden;
	background: rgba(0,0,0,0.5);
	transition: all 0.3s;
}
.md-modal-show  ~ .md-overlay {
	opacity: 1;
	visibility: visible;
}
</style>
