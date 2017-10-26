<template>
	<div>
		<ul class="movies-list">
			<li class="movies-item list-group-item list-group-item-action" v-for="movie in movies" @click="showModal(movie)"><span class="movie-title">{{ movie.title }}</span><span class="movie-rate">&#9733; <b>{{ movie.vote_average }}</b></span></li>
		</ul>

		<movie-modal :show="isShown" :backImage="chosenMovie.backdrop_path" @modalClosed="closeModal()">
			<div class="modal-poster col-12 col-sm-5">
				<img :src="chosenMovie.poster_path ? 'https://image.tmdb.org/t/p/w300' + chosenMovie.poster_path : '#'" :alt="chosenMovie.title" :title="chosenMovie.title">
			</div>
			<div class="modal-info col-12 col-sm-7">
				<h2>{{chosenMovie.title}}</h2>
				<p>
					<h2 class="modal-rate">&#9733; <b>{{ chosenMovie.vote_average }}</b><span class="rate-count"> ({{ chosenMovie.vote_count }})</span></h2>
				</p>
				<p><b>Release date</b><br><span class="modal-date success">{{ chosenMovie.release_date }}</span></p>
				<p><b>Genres</b><br>
					<span class="modal-genres">
						<a :href="'https://www.themoviedb.org/genre/' + genre.id" class="badge badge-success movie-genre" v-for="genre in chosenMovie.genres">{{ genre.name }}</a>
					</span>
				</p>
				<p><b>Overview</b>
					<p>{{ chosenMovie.overview }}</p>
				</p>
			</div>
		</movie-modal>

	</div>
</template>

<script>
import movieModal from "./movieModal.vue"

export default {
	props: ['movies'],
	components: {
		'movieModal': movieModal
	},
	data () {
		return {
			isShown: false,
			chosenMovie: {}
		}
	},
	methods: {
		showModal: function(movie) {
			this.chosenMovie = movie;
			this.isShown = !this.isShown;
		},
		closeModal: function(value) {
			this.isShown = value;
		}
	}
}
</script>

<style>
.modal-info p span {
	font-size: 0.9em;
}

.movies-item {
	cursor: pointer;
	transition: all .3s ease;
	padding: 0.4rem 1rem;
	text-align: left;
}
.modal-poster {
	margin-bottom: 20px;
}
.modal-info {
	text-align: left;
}
.modal-rate {
	color: #3CD37C;
	font-size: 1.8em;
	margin-bottom: 10px;
}
.rate-count {
	font-size: 12px;
	color: #999;
}
.movie-genre {
	margin-right: 5px;
	padding: 4px;
}
.movie-rate {
	float: right;
	color: #3CD37C;
}
.movies-list {
	padding: 0;
}
</style>
