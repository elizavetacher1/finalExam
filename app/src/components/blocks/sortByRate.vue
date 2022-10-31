<template>
<div class="additional">
    <div class="inputsWrap"
    >
        <b-dropdown id="dropdown-1" text="BY RATE" class="m-md-2"
        >
            <b-dropdown-form
            >
                <b-form-input
                    class="one"
                    type="range"
                    min="1"
                    max="10"
                    step="1"
                    @change="setPricesReverse"
                    v-model.number="minRate"/>
                <b-form-input
                    class="two"
                    type="range"
                    min="1"
                    max="10"
                    step="1"
                    value="3"
                    @change="setPricesReverse"
                    v-model.number="maxRate"/>
            
                <div class="wrapFprRateRes"
                    
                >
                    <p>  Minimum movie rate: {{this.minRate}}  </p>
                    <p>  Maximum movie rate: {{this.maxRate}}  </p>
                    <b-col lg="4" class="btnSearch" 
                    @click="searchRatedMovies(); removePopMov(); $emit('setfilter', 'rate')" >
                    <b-button size="lg"
                    class="rateSearch">
                    SEARCH
                    </b-button></b-col>
                </div>
            </b-dropdown-form> 
        </b-dropdown>
    </div>

    <b-container b-container fluid> 
        <b-row class="absolute margin marginLeft  gx-5 gx-lg-6">
            <b-col  md="3" lg="2" class="cursor popular my-lg-3 my-sm-1 px-2"  
                v-for="el in moviesByRate" 
                :key="el.id"
                @click="getIdOfMovie(el.id); showModal = !showModal"
                >
                <img v-if="el.poster_path != null" :src="'https://image.tmdb.org/t/p/w500/' + el.poster_path" 
                :title="el.title"
                >
                <img v-else src='@/assets/noImage.jpg'>
                <span class="rate"> {{el.vote_average*10}}<small> % </small> </span>
                <h3 class="title"> {{el.title}} </h3>
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
    name: 'MoviesByRate',
    props: ['filter'],
    components: {modalPage},
    data () {
        return {
            moviesByRate: [],
            minRate: 4,
            maxRate: 6,     
            showModal: false,
            movieDetails: [],
        }
    },
    methods: {
        setPricesReverse () {
            if (this.minRate > this.maxRate) {
                let opt = this.maxRate;
                this.maxRate = this.minRate;
                this.minRate = opt
            }
        },
        searchRatedMovies () {
            this.moviesByRate = []
                Promise.all ([
                fetch('https://api.themoviedb.org/3/discover/movie?api_key=a85812a66a606a68d513d8e167b2963f&page=1&vote_average.gte=' + this.minRate + '&vote_average.lte=' + this.maxRate).then(resp => resp.json()),
                fetch('https://api.themoviedb.org/3/discover/movie?api_key=a85812a66a606a68d513d8e167b2963f&page=2&vote_average.gte=' + this.minRate + '&vote_average.lte=' + this.maxRate).then(resp => resp.json()),
                fetch('https://api.themoviedb.org/3/discover/movie?api_key=a85812a66a606a68d513d8e167b2963f&page=3&vote_average.gte=' + this.minRate + '&vote_average.lte=' + this.maxRate).then(resp => resp.json()),
                ]).then((res)=>{
                    for (const item of res) {
                        this.moviesByRate.push(...item.results)                             
                        } 
                })  
                  
        },
        resetMoviesByRate () {
            this.moviesByRate = []
        },
        backPop () {
            this.$emit('backPop')
        },
        removePopMov () {
            this.$emit('removePopMov')
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
    computed: {
        currentfilter() {
            return this.filter
        }
    },
    watch: {
        currentfilter () {
            if (this.filter != 'rate') {
                this.resetMoviesByRate ()
            }
        }
    }
}


</script>

<style lang="scss" scoped>

img:hover{
    opacity: 0.6;
    -webkit-filter: contrast(200%);  
}

.rateSearch {
    margin-top: 0;
    background: #2e262636;
    padding: 2px;
    font-size: 20px;
}

li {
    height: 300px;
    width: 735px;
}

.wrapFprRateRes {
    margin-top: 53px;
    margin-left: -210px;
    font-weight: 600;
        & p {
           font-size: 25px;  
           color: #1a0f0f;
           margin-bottom: 0 !important;
            @media (max-width: 900px) {
                font-size: 15px;
            }

        }
    @media (max-width: 900px) {
        margin-left: -168px;
    }
}

input[type=range] {
    -webkit-appearance: none;
    width: 430px;
    position: absolute;
    left: -242px;
    top: 25px;
    height: 15px;
        @media (max-width: 900px) {
        width: 200px;
        left: -136px;
    }
}

input[type=range]::-webkit-slider-thumb {
  -webkit-appearance: none;
  height: 35px;
  width: 35px;
  border-radius: 25px;
  position: relative;
  z-index: 4;
  cursor: pointer;
  background: #6c936c;
  border: 1px solid black;
    @media (max-width: 900px) {
        height: 20px;
        width: 20px;
    }
}

input {
    border: 1px solid #00000042;
    height: 19px;
    border-radius: 25px;
    background: #935e5e2b;
}

.range-slider {
    position: relative;
}

.two {
    position: absolute;
    left: 0;
    top: 2px;
}

.inputsWrap {
    width: 200px;
}

.marginLeft {
    margin-left: -1206px;
        @media (max-width: 1500px) {
            margin-left: -509px;
        }
}

</style>