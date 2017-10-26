<template>
	<div id="app" class="container">
		<div>
			<h1 class="header">Movie Searcher</h1>
			<movie-search v-on:filtered="setFilterValue"></movie-search>
			<br>
			<movie-list :movies="filterMovies"></movie-list>
		</div>
		
	</div>
</template>

<script>
import tmdb from "./lib/themoviedb/themoviedb.js"
import movieSearch from "./components/movieSearch.vue"
import movieList from "./components/movieList.vue"


var themoviedb = tmdb;
themoviedb.common.api_key = "e8a478369d5bf5494bb85b2dd09b5667";

export default {
	name: 'app',
	components: {
		'movieSearch': movieSearch,
		'movieList': movieList,
	},
	data () {
		return {
			movies: [],
			allGenres: {},
			filterValue: '',
		}
	},
	computed: {
		filterMovies: function() {
			let filterText = this.filterValue.trim()
			return this.movies.filter(function(movie) {
				return movie.title.toLowerCase().match(filterText) != null
			})
		}
	},
	methods: {
		getMovies: function() {
			var self = this

			themoviedb.movies.getTopRated({},
				function (movies) {
					movies = JSON.parse(movies)
					if (movies.results && movies.results.length > 0) {
						self.movies = movies.results;
					}
				}, 
				function (error) {
					// do something with errorCallback
					console.error(error)
				})

		},

		setFilterValue: function(value) {
			this.filterValue = value;
		},

		getGenres: function() {
			themoviedb.genres.getList({}, (data) => {
				var genres = JSON.parse(data).genres

				if (!genres) {
					return
				}

				var genresObj = {}
				genres.forEach((value, index, array) => {
					genresObj[value.id] = value.name 
				})

				this.allGenres = genresObj;
				// console.log(this.allGenres)
				this.addGenreNames();
			}, 
			function(error) {console.error(error)})
		},

		addGenreNames: function() {
			// console.log('this.movies')
			var that = this
			this.movies.forEach(function(movie) {
				
				movie.genres = movie.genre_ids.map(function(id) {
					return { id: id, 'name': that.allGenres[id] } 
				})
			})
			
		}
	},
	created: function() {
		this.getMovies();
		this.getGenres();
	}
}
</script>

<style>
body {
	background-color: #414650;
}
#app {
	font-family: 'Avenir', Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #b4bec8;
	margin-top: 60px;
}

img {
	width: 100%;
	height: auto;
}

</style>
