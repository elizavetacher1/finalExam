<template>
    <div class="modalPage" >
        <p class="resetForDetails" 
        @click="cancelModal()"
        title="Reset details"
        > 
        </p> 
        <h3 class="title"> {{movieDetails.title}} </h3>
        <div class="movieDetailsFlex">
          <img v-if="movieDetails.poster_path != null" 
          :src="'https://image.tmdb.org/t/p/w500/' + movieDetails.poster_path" 
          :title="movieDetails.title"
          >
          <img v-else src='@/assets/noImage.jpg'
          :title="movieDetails.title"> 
          <div class="overview"> <h2>WHAT is this movie about?</h2>
           
          {{movieDetails.overview}}
         
          <p class="releaseDate"> Release date: {{movieDetails.release_date}}</p>
          </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'movieDetails',
    props: ['movieDetails', 'showDetails'],
    methods: {
      cancelModal () {
        this.$emit ('cancelModal')
      }
    }
}

</script>

<style lang="scss" scoped>

h3 {
  margin-bottom: 15px;
}

.modalPage {
    position: fixed;
    z-index: 3;
    width: 100vw;
    display: flex;
    top: 0;
    left: 0;
    flex-direction: column;
    background: rgba(2, 1, 1, 0.75);
    padding: 75px 0 950px;
    text-align: center;
      & img {
        width: 450px;
        height: 680px;
        border-radius: 0;
        @media (max-width: 900px) {
          display: none;
        }
      }
    @media (max-width: 900px) {
        top: -55px;
    }
}

.title {
    text-shadow: -3px 2px 1px #a06363;
    color: #ffffff;
    padding-top: 4px;
    font-size: 45px;
}

.resetForDetails {
    cursor: pointer;
    position: relative;
    margin-left: 778px;
  /*  position: absolute;*/
    border: none;  
    border: none;
    }

.resetForDetails:before, .resetForDetails:after {
  content: ""; 
  position: absolute;
  background:  #b19595; 
  top: 0;
    @media (max-width: 2600px) {
      
    }
}

.resetForDetails:before {
  transform: rotate(45deg) }

.resetForDetails:after {
  transform: rotate(-45deg) } 


</style>

<style lang="scss" scoped>

.releaseDate {
    color: #483c2b;
    text-shadow: 1px 1px 1px black;
    text-align: end;
    position: absolute;
    bottom: 0;
}

h2 {
  margin-bottom: 30px;
  }

.movieDetailsFlex {
  flex-direction: row;
  display: flex;
  flex-direction: row;
  margin: 0 auto;
  position: relative;
  bottom: 0;
    & .overview {
    width:  470px;
    height: 680px;
    background: rgb(255 255 255 / 63%);
    text-align: initial;
    font-family: future;
    color: #483c2b;
    text-shadow: 1px 1px 1px black;
    padding: 26px;
    font-size: 20px;
    }
  @media (max-width: 900px) {
    display: flex;
    flex-direction: column;
  }
}

.resetForDetails:before, .resetForDetails:after {
    width: 45px;
    height: 10px;
}

.resetForDetails:hover:before {
    background: #897777;
}

.resetForDetails:hover:after {
    background: #897777;
}




</style>
