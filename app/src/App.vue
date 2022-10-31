<template>
<div class="appProject">
  <headerComp
  @openTVShows="openTVShows"
  @openMovies="openMovies"
  />
  <mainComp 
    v-if="openMovMark == true"    
    :allPopular="allPopular">
  </mainComp>
  <tvShowsComp
    v-if="openTVMark == true"
    :allTVShows="allTVShows">
  </tvShowsComp>
  <footerComp></footerComp>
</div>
</template>

<script>

import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

import headerComp from '@/components/layouts/headerComp'
import tvShowsComp from '@/components/layouts/tvShowsComp'
import mainComp from '@/components/layouts/mainComp'
import footerComp from  '@/components/layouts/footerComp'


export default {
    name: 'App', 
    components: {
    headerComp, mainComp, tvShowsComp, footerComp
    }, 
    data () {
      return {
        allPopular: [],
        allTVShows: [],
        openMovMark: true,
        openTVMark: false
      }
    },
     beforeCreate() {
      Promise.all ([
          fetch('https://api.themoviedb.org/3/movie/popular?api_key=a85812a66a606a68d513d8e167b2963f&page=1')
          .then(resp => resp.json()),
          fetch('https://api.themoviedb.org/3/movie/popular?api_key=a85812a66a606a68d513d8e167b2963f&page=2')
          .then(resp => resp.json()),
          fetch('https://api.themoviedb.org/3/movie/popular?api_key=a85812a66a606a68d513d8e167b2963f&page=3')
          .then(resp => resp.json())]).
          then((res)=>{
            for (const item of res) {
              if(item.results){
                this.allPopular.push(...item.results);
              }                              
            }
          }
        ),
      Promise.all ([
        fetch('https://api.themoviedb.org/3/tv/popular?api_key=a85812a66a606a68d513d8e167b2963f&language=en-US&page=1')
        .then(resp => resp.json()),
        fetch('https://api.themoviedb.org/3/tv/popular?api_key=a85812a66a606a68d513d8e167b2963f&language=en-US&page=2')
        .then(resp => resp.json()),
        fetch('https://api.themoviedb.org/3/tv/popular?api_key=a85812a66a606a68d513d8e167b2963f&language=en-US&page=3')
        .then(resp => resp.json())]).
        then((res)=>{
          for (const item of res) {
            if(item.results){
              this.allTVShows.push(...item.results);
            }                              
          }
        }
      )           
     },
  methods: {
    openMovies () {
      this.openMovMark = true;
      this.openTVMark = false
    },
    openTVShows () {
      this.openTVMark = true;
      this.openMovMark = false;
    }
  }
}

</script>

<style lang="scss">

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.appProject {
    background: url(@/assets/betman.jpg); 
    background-size: cover;
    background-attachment: fixed;
    height: 7000px;
    width: 100%; 
        @media (max-width: 900px) {
            height: 9000px;
        }
        @media (max-width: 750px) {
            height: 15000px;
        }
        @media (max-width: 400px) {
            height: 28000px;
        }
} 


</style>
