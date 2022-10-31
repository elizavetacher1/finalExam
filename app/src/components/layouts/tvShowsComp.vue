<template>

<div class="tvShows">
    <b-container fluid> 
        <b-row  class="justify" >
            <b-col sm="6" md="3" lg="2" class="cursor popular my-lg-3 my-md-1 px-2"  
                v-for="item in this.allTVShowsResults" :key="item.id" 
                @click="getIdOfShow(item.id); showModal = !showModal"
                > 
                <img v-if="item.poster_path != null" 
                :src="'https://image.tmdb.org/t/p/w500/' + item.poster_path" :title="item.name">
                <img v-else src='@/assets/noImage.jpg'
                :title="item.name"> 
                <span class="rate"> {{item.vote_average*10}}<small> % </small> </span>
                <h3 class="title"> {{item.name}} </h3>
            </b-col>
        </b-row>
    </b-container>  
</div>

</template>

<script>


export default {
    name: 'episodsComp',
    props: ['allTVShows'],
    data () {
        return {
            allTVShowsResults: this.allTVShows,
            showDetails: [],
            showModal: false,
        }
    },
    methods: {        
        getIdOfShow (showId) {
            fetch("https://api.themoviedb.org/3/tv/" + showId + "/season/1/episode/1?api_key=a85812a66a606a68d513d8e167b2963f")
               .then(response => response.json())
               .then(json => this.showDetails = json);
                var showDetails = this.showDetails
                return showDetails
        },
    }
}

</script>

<style lang="scss" scoped>

    .popular {
        @media (max-width: 560px) {     
            width: 280px;
            height: 440px;
        }
    }

    .ds {
        color:red;
        background: grey;
    }

    .tvShows {
        padding: 0 70px
    }


</style>