<template>

  <div class="navMenu">
    <b-navbar  type="dark" variant="dark">
      <b-navbar-nav bg-light>
        <b-nav-item bg-light @click="openMovies()">Movies</b-nav-item>
        <b-nav-item bg-light 
          @click="openTVShows()"
          class="noWrap"
          >
        TV-Shows</b-nav-item>
      </b-navbar-nav>
    </b-navbar>

  </div>
</template>

<script>



export default {
  name: 'headerComp',
  data: () => {
    return {
      
    }
  },
  methods: {
    openTVShows () {
      this.$emit('openTVShows')
    },
    openMovies () {
      this.$emit('openMovies')
    }
  },
}

</script> 

<style scoped>

.navMenu {
  color: white;
  height: 200px;
  margin: 0 auto;
  letter-spacing: 4px;
}

nav .navbar-nav {
  justify-content: center;
  width: 100%;
}

li {
  font-size: 20px;
  padding: 0 20px;
  font-family: Georgia, serif;
}

</style>

<style lang="scss" scoped>

  .noWrap {
    white-space: nowrap;
}

</style>