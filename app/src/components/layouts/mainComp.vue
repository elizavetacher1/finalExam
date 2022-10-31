<template>
    <div class="wrapBackgro"
    >  
    <div class="general">
        <searchBlock 
        :filter="filterBy"
        :movieDetails="movieDetails"
        :allPopular="allPopular"
        @removePopMov="removePopMov" 
        @backPop="backPop"
        @setfilter="filterBy = $event"
        @notShowFilter="notShowFilter" 
        />
        <filterMenu
        v-if="ShowFilterMark"  
        @setfilter="filterBy = $event" 
        @removePopMov="removePopMov" @backPop="backPop"/> 
        </div> 
            <b-container fluid> 
                <b-row  class="justify" >
                    <b-col sm="6" md="3" lg="2" class="cursor popular my-lg-3 my-md-1 px-2"  
                        v-for="(item) in this.allPopularwithRes" :key="item.id" 
                        @click="getIdOfMovie(item.id); showModal = !showModal"> 
                        <img v-if="item.poster_path != null" 
                        :src="'https://image.tmdb.org/t/p/w500/' + item.poster_path" :title="item.title">
                        <img v-else src='@/assets/noImage.jpg'
                        :title="item.title"> 
                        <span class="rate"> {{item.vote_average*10}}<small> % </small> </span>
                        <h3 class="title"> {{item.title}} </h3>
                    </b-col>
                </b-row>
            </b-container>  
        <modalPage 
        v-if="showModal"
        @cancelModal="cancelModal"
        :movieDetails="movieDetails"/>
    </div>
</template>

<script>
import searchBlock from '@/components/blocks/searchBlock'
import filterMenu from '@/components/blocks/filterMenu'
import modalPage from '@/components/blocks/modalPage'

export default {
    name: 'mainComp',
    components: {
        filterMenu,
        searchBlock,
        modalPage
    },
    props: ['allPopular'],
    data: function () {
        return {
            movieDetails: [],
            searchMovie: '',
            allPopularwithRes: this.allPopular,
            showModal: false,
            filterBy: '',
            ShowFilterMark: true
        }
    },
    methods: {        
        getIdOfMovie (x) {
            fetch("https://api.themoviedb.org/3/movie/" + x + "?api_key=a85812a66a606a68d513d8e167b2963f")
               .then(response => response.json())
               .then(json => this.movieDetails = json);
                var movieDetails = this.movieDetails
                return movieDetails  
        },
        allPopNotShow () {
           this.allPopular1 = []
        },
        removePopMov () {
            this.allPopularwithRes = []
        },
        backPop () {
            this.allPopularwithRes=this.allPopular
        },
        notShowFilter () {
            this.ShowFilterMark = !this.ShowFilterMark
        },
        cancelModal () {
            this.showModal = !this.showModal
        }
    }
}
</script>

<style  lang="scss">

    .justify {
        justify-content: center;
    }

    .h3 {
        white-space: none !important;
        padding: 40px 0;
        color: white;
    }

    .cursor {
        cursor: pointer;
    }

    .popular {
        position: relative; 
        display: flex;
        flex-direction: column;
        --bs-gutter-y: -1rem;
        @media (max-width: 900px) {
            --bs-gutter-y: 2rem;
        }
    }

    .title {
        padding-top: 15px;
        line-height: 1;
        text-shadow: 2px 2px 4px #000000;
        color: #201b1b;
        font-family: georgia serif;
        text-align: center;
        color: white;
        font-size: 28px;
            @media (max-width: 1500px) {
                font-size: 25px;
        }
    }

    img {
        width: 100%;
        border-radius: 0 0 60px 0;
        box-shadow: -9px -10px 23px 0px rgb(0 0 0);
        @media (max-width: 1500px) {
            height: 322px;
        }
        @media (max-width: 765px) {
            
            height: 363px;
            margin: 0 auto;
        }
    }
    
    .rate {
        border-radius: 50px;
        background: rgba(0, 0, 0, 0.5294117647);
        width: 41px;
        height: 39px;
        position: absolute;
        color: #fff8f8;
        font-weight: bold;
        text-align: center;
        top: 1px;
        text-shadow: 1px 1px black;
        font-size: 15px;
        left: 11px;
        padding: 6px 10px 0 0;
    }

    small {
        font-size: 10px;
        position: absolute;
        top: 5px;
    }

</style>


<style lang="scss" scoped>

.general {
    display: flex;
    flex-direction: column;
    margin-bottom: 0;
        @media (max-width: 1500px) {
        margin-bottom: 300px;
    }
}

.wrapBackgro {
    background: rgb(225 209 205 / 60%);
    max-width: 1500px;
    margin: 0 auto;
    height: 5000px;
    padding: 0 35px;
        @media (max-width: 900px) {
            height: 7000px;
        }
        @media (max-width: 750px) {
            height: 14000px;
        }
        @media (max-width: 400px) {
            height: 27500px;
        }
}

ul, li {
    list-style: none;
}

.flex {
    display: flex;
}

img:hover{
    opacity: 0.6;
    -webkit-filter: contrast(200%);  
}

</style>







