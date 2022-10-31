<template>
<div @click="showDropDownList = true">

    <b-dropdown id="dropdown-1" text="GENRES" class="m-md-2"
    >
        <div class="dropdownList"
        v-if="showDropDownList"
        > 
            <b-dropdown-item v-for="genre in genresButton.genres" :key="genre.id"  
                @click="getIdOfGenre(genre.id); removePopMov();
                $emit('setfilter', 'genres')"

                >
                {{genre.name}}
            </b-dropdown-item>

        </div>
    </b-dropdown>

    <b-container b-container fluid class="marginLeft"> 
            <b-row class="absolute margin mainPop gx-5 gx-lg-6">
                <b-col  md="3" lg="2" class="cursor popular my-lg-3 my-sm-1 px-2"  
                    v-for="movie in movieByGenre" :key="movie.id"
                       @click="getIdOfMovie(movie.id);
                        showModal = !showModal;"
                    >
                    <img v-if="movie.poster_path != null" :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path" 
                    :title="movie.title"
                    >
                    <img  v-else src='@/assets/noImage.jpg'>
                    <span class="rate"> {{movie.vote_average*10}}<small> % </small> </span>
                    <h3 class="title"> {{movie.title}} </h3>
                </b-col>
            </b-row>
    </b-container>
    <modalPage 
        :movieDetails="movieDetails"
        @cancelModal="cancelModal" 
        v-if="showModal"/> 
</div>
</template>

<script>


import modalPage from '@/components/blocks/modalPage'

export default {
    name: 'genresButtons',
    components: {modalPage},
    props: ['filter'],
    data () {
        return {
            genresButton: [],
            movieByGenre: [],
            showModal: false,
            movieDetails: [],
            showDropDownList: true
        }
    },
    methods: {
        getIdOfGenre (genreId) {
            this.movieByGenre = []
            Promise.all ([
                fetch("https://api.themoviedb.org/3/discover/movie?api_key=a85812a66a606a68d513d8e167b2963f&page=1&with_genres=" + genreId)
                    .then(resp => resp.json()),
                fetch("https://api.themoviedb.org/3/discover/movie?api_key=a85812a66a606a68d513d8e167b2963f&page=2&with_genres=" + genreId)
                    .then(resp => resp.json()),
                fetch("https://api.themoviedb.org/3/discover/movie?api_key=a85812a66a606a68d513d8e167b2963f&page=3&with_genres=" + genreId)
                    .then(resp => resp.json())]).then((res)=>{
                    if (this.movieByGenre.length <= 3) {
                        for (const item of res) {
                            this.movieByGenre.push(...item.results)                            
                        }
                    }     
                }
            )          
        },
        removePopMov () {
            this.$emit('removePopMov')
        },
        backPop () {
            this.$emit('backPop')
        },
        resetMoviesByGenre () {
            this.movieByGenre = []
        },
        getIdOfMovie (x) {
            fetch("https://api.themoviedb.org/3/movie/" + x + "?api_key=a85812a66a606a68d513d8e167b2963f")
               .then(response => response.json())
               .then(json => this.movieDetails = json);
                var movieDetails = this.movieDetails
                return movieDetails  
        },
        cancelModal () {
            this.showModal = !this.showModal
        }
    },
    beforeCreate() {
        fetch("https://api.themoviedb.org/3/genre/movie/list?api_key=a85812a66a606a68d513d8e167b2963f&language=en-US")
            .then(response => response.json())
            .then(json => this.genresButton = json);
    },
    computed: {
        currentfilter() {
            return this.filter
    }
    },
    watch: {
        currentfilter () {
            if (this.filter != 'genres') {
                this.resetMoviesByGenre ()
            }
        }
    },  
}
</script>

<style lang="scss">

.reset {
  cursor: pointer; 
  position: relative;  }

.reset:before, .reset:after {
  content: ""; 
  position: absolute; 
  background: gray; }

.reset:before {
  transform: rotate(45deg) }

.reset:after {
  transform: rotate(-45deg) } 

.dropdown-toggle::after {
    vertical-align: 0 !important;
    position: absolute;
    top: 78%;
    left: 41%;
    width: 20px;
    height: 20px;
} 

#dropdown-1 {
    position: relative;
}


.dropdown-menu.show {
    position: relative;
    border: none;
    background: transparent;
}

.dropdown-item {
    background: none;
    background-color: none !important;
    width: 100%;
    --bs-dropdown-link-active-bg: none;
    --bs-dropdown-link-hover-bg: none;
    text-align: center;
}

</style>

<style lang="scss" scoped>

a {
    padding: 2px 7px;
}

img:hover{
    opacity: 0.6;
    -webkit-filter: contrast(200%);  
}

.marginLeft {
    margin-left: -1027px;
        @media (max-width: 1500px) {
            margin-left: -328px;
        }
}

.dropdownList {
    width: 804px;
    flex-wrap: wrap;
    justify-content: center;
    display: flex;
    position: absolute;
    left: -225%;
        @media (max-width: 1500px) {
            width: 728px;
        }
        @media (max-width: 900px) {
            width: 400px;
        }
}
  
#dropdown-1__BV_toggle_ {
    color: none !important;
}

li {
    border: 1px solid grey;
    border-radius: 9px;
    margin: 3px;
    font-size: 25px; 
    box-shadow: 4px 2px 3px black;
    text-shadow: 1px 1px 1px black;
    @media (max-width: 1500px) {
        font-size: 18px
    }
    @media (max-width: 900px) {
        font-size: 18px
    }
}

</style>
