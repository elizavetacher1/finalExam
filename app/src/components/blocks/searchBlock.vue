<template>
<div>
    <div class="searchBlock">
        <input 
            class="formSearch"
            placeholder="Find a movie..." 
            aria-label="Search" 
            v-model="search">
        
        <b-col lg="4" class="btnSearch" 
            @click="searchMethod(); removePopMov(); test = !test;
            $emit('setfilter', 'search');
            notShowFilter()"
            >
            <b-button size="lg">
        SEARCH
        </b-button></b-col>
        <b-col lg="4" 
            @click="resetMoviesBySearch(); backPop(); notShowFilter()"
            >
            <b-button size="lg"
            class="resetSearchResults">
            Remove Results
        </b-button></b-col> 
        <input 
        v-model="nores" 
        class="nores" 
        disabled/>    
    
    </div>
    <b-container b-container fluid > 
             <b-row  class="justify" >
                <b-col  md="3" lg="2" class="cursor popular my-lg-3 my-sm-1 px-2"    
                v-for="el in searchMovieData" :key="el.id" 
                @click="getIdOfMovie(el.id); showModal = !showModal">
                    <img v-if="el.poster_path != null" :src="'https://image.tmdb.org/t/p/w500/' + el.poster_path" 
                    :title="el.title"
                    >
                    <img  v-else src='@/assets/noImage.jpg'>
                    <span class="rate"> {{el.vote_average*10}}<small> % </small> </span>
                    <h3 class="title"> {{el.title}} </h3>
            </b-col>
        </b-row>
    </b-container>
    <modalPage 
        @cancelModal="cancelModal"
        @pik="pik()"
        v-if="showModal"
        :movieDetails="movieDetails"/>
</div>

</template>

<script>

import modalPage from '@/components/blocks/modalPage'

export default {
    name: 'searchBe',
    components: {modalPage},
    props: ['allPopular', 'filter'],
    data () {
        return {
            searchMovieData: [],
            search: '',
            nores: '',
            movieDetails: [],
            showModal: false,
        }
    },
   methods: {
    searchMethod () {
        this.searchMovieData = []
        Promise.all ([
                fetch('https://api.themoviedb.org/3/search/movie?api_key=a85812a66a606a68d513d8e167b2963f&query=' + this.search + '&page=1').then(resp => resp.json()),
                fetch('https://api.themoviedb.org/3/search/movie?api_key=a85812a66a606a68d513d8e167b2963f&query=' + this.search + '&page=2').then(resp => resp.json()),
                fetch('https://api.themoviedb.org/3/search/movie?api_key=a85812a66a606a68d513d8e167b2963f&query=' + this.search + '&page=3').then(resp => resp.json())
                ]).then((res)=>{
                    for (const item of res) {
                        this.searchMovieData.push(...item.results)                      
                    }    
                    if (this.searchMovieData.length <= 0 ) {
                        this.nores = 'No matches for "' + this.search + '"'
                    } 
                })  
                  
    },
    resetMoviesBySearch () {
        this.searchMovieData = []
        this.nores=''
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
    pik () {
        this.movieDetails = []
    },
    cancelModal () {
       this.showModal = !this.showModal
    },
    notShowFilter () {
        this.$emit ('notShowFilter')
    }
   },
    computed: {
        currentfilter() {
            return this.filter
    }
    },
    watch: {
        currentfilter () {
            if (this.filter != 'search') {
                this.resetMoviesBySearch ()
            }
        }
    }, 
   
}
    
</script>

<style lang="scss">

.resetSearchResults {
    background: #6c936c !important;
    @media (max-width: 1500px) {
        height: 70px !important;
    }
}

.searchBlock {
    position: relative;
    padding: 60px 20px 0px 0;
    @media (max-width: 1500px) {
            display: flex;
            flex-direction: row;
            height: 120px;
            padding-top: 10px;
            padding-bottom: 10px;
    }
    @media (max-width: 900px) {
            flex-direction: column;
    }
}     

.nores{
     border: none;
    font-style: italic;
    background: transparent;
    font-size: 20px;
    color: white;
    text-shadow: 1px 1px 1px black;
    padding: 40px 50px 40px 9px;
}

.formSearch {
    transition: width 1s linear;
    font-family: futura;
    font-size: 25px; 
    width: 300px;
    border-radius: 0.375rem;
    border: none;
    outline: none;
    margin-bottom: 5px;
    padding: 10px 40px 10px 30px;
        &:focus {
            width: 600px;
            @media (max-width: 1500px) {
                width: 600px;
            }
            @media (max-width: 900px) {
                width: 300px;
            }
        }
        @media (max-width: 1500px) {
            height: 70px;
            width: 600px;
        }
        @media (max-width: 900px) {
            height: 100%;
            width: 300px;
        }
}

.btn-lg {
    height: 100%;
    font-size: 20px;
    padding: 10px 40px 10px 40px;
    text-shadow: 6px 7px 13px black;
}

ul {
    list-style: none;
    font-family: Futura;
    font-size: 21px;
}

</style>

<style lang="scss" scoped>

.btn-lg {
    margin-top: 10px;
        @media (max-width: 1500px) {
            height: 70px;
            margin-top: 0;
            padding: 5px;
            width: 150px
        }
        @media (max-width: 900px) {
            height: 40px;
        }

}

.btnSearch {
    width: 150px;
}

.col-lg-4 {
    width: 150px;
}

img:hover{
    opacity: 0.6;
    -webkit-filter: contrast(200%);  
}

</style>
