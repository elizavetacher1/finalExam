<template>
    <div v-bind:class="{ForvDropItWidth: ForvDropItWidth}">

    <b-dropdown id="dropdown-1" text="RELEASE YEAR" class="m-md-2">
        <div class="dropdownList"> 
            <b-dropdown-item v-for="(option, ind) in years" :key="ind"
                @click="sortByYear(option); removePopMov(); ForvDropItWidth = true
                $emit('setfilter', 'year')"
                v-on:click="ForvDropItWidth = true" >
                     {{ option }}
           </b-dropdown-item>
        </div>
    </b-dropdown>
    <b-container b-container fluid class="marginLeft"> 
        <b-row class="absolute margin mainPop gx-5 gx-lg-6">
            <b-col  md="3" lg="2" class="cursor popular my-lg-3 my-sm-1 px-2" 
                v-for="item in movieByYear" 
                    :key="item.id"
                    @click="getIdOfMovie(item.id); showModal = !showModal"
                    >
                    <img v-if="item.poster_path != null" :src="'https://image.tmdb.org/t/p/w500/' + item.poster_path" 
                    :title="item.title"
                    >
                    <img v-else src='@/assets/noImage.jpg'>
                    <span class="rate"> {{item.vote_average*10}}<small> % </small> </span>
                    <h3 class="title"> {{item.title}} </h3>
                </b-col>
        </b-row>
    </b-container>

        <modalPage 
            :movieDetails="movieDetails"
            v-if="showModal"
            @cancelModal="cancelModal"
            />

    </div> 
</template>

<script>


import modalPage from '@/components/blocks/modalPage'

export default {
    name: 'niceCompo',
    components: {modalPage},
    props: ['filter'],
    data() {
        return {
            options: [],
            selected: '',
            movieByYear: [],
            ForvDropItWidth: false,
            showModal: false,
            movieDetails: [],
            test: []
        }
    },
    computed: {
        years: function () {
            let array = []
            for (let i = 2010; i<=2022; i++) {
                 array.push(i)
                 array = this.options
                }
        return this.options
            },
        currentfilter() {
            return this.filter
            }   
        },

    watch: {
        currentfilter () {
            if (this.filter != 'year') {
                this.resetMoviesByYear ()
            }
        }
    },
    methods: {
        sortByYear (e) {
            this.movieByYear = []
            Promise.all ([
                fetch('https://api.themoviedb.org/3/discover/movie?api_key=a85812a66a606a68d513d8e167b2963f&page=1&primary_release_year=' + e).then(resp => resp.json()),
                fetch('https://api.themoviedb.org/3/discover/movie?api_key=a85812a66a606a68d513d8e167b2963f&page=2&primary_release_year=' + e).then(resp => resp.json()),
                fetch('https://api.themoviedb.org/3/discover/movie?api_key=a85812a66a606a68d513d8e167b2963f&page=3&primary_release_year=' + e).then(resp => resp.json())
                ]).then((res)=>{
                    if(this.movieByYear.length <= 3) {
                        for (const item of res) {
                            if(res.length){
                                this.movieByYear.push(...item.results);         
                            }                                
                        }
                    }     
                }
            )
        },

       backPop () {
            this.$emit('backPop')
        },
        removePopMov () {
            this.$emit('removePopMov')
        },
        resetMoviesByYear () {
            this.movieByYear = []
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
    }
}

</script>

<style lang="scss" scoped>

a {
    padding: 2px 12px;
}

img:hover{
    opacity: 0.6;
    -webkit-filter: contrast(200%);  
}

.dropdownList {
    width: 634px;
    display: flex;
    flex-wrap: wrap;
    margin-left: -185px;
        @media (max-width: 1500px) {
             margin-left: 30px;
             width: 350px;
        }
}

li {
    border: 1px solid grey;
    border-radius: 9px;
    margin: 5px;
    font-size: 29px;
    box-shadow: 4px 2px 3px black;
    text-shadow: 1px 1px 1px black;
    @media (max-width: 1500px) {
        font-size: 20px
    }
    @media (max-width: 900px) {
    font-size: 15px
    }
}

.form-select {
    width: 300px !important;
}

.marginLeft {
    margin-left: -749px;
        @media (max-width: 1500px ) {
            margin-left: -52px;
        }
}

option {
    width: 500px
}

</style>