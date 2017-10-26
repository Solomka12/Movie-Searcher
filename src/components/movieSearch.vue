<template>
	<div>
		<input class="form-control form-control-lg" type="text" placeholder="Filter movies" v-model="inputValue" @input="filterMovies">
	</div>
</template>

<script>
export default {
	data () {
		return {
			inputValue: ''
		}
	},
	methods: {
		filterMovies: function() {
			this.$emit('filtered', this.inputValue)
		}
	}
}
</script>

<style>

</style>
