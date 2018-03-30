<template lang="pug">
#app
  h1 {{ msg }}
  select(v-model="selectedCountry") 
    option(v-for="country in countries" :value="country.value") {{country.name}}

  spinner(v-show="loading")  

  ul
    artist(v-for="artist in artists" 
    v-bind:artist="artist" v-bind:key="artist.mbid")
    
</template>

<script>
import Artist from './components/Artist.vue'
import Spinner from './components/Spinner.vue'
import getArtists from "./api"

export default {
  name: 'app',
  data () {
    return {
      msg: 'VueMusic',
      artists: [],
      countries : [
        {name: 'Argentina', value:'argentina'},
        {name: 'Colombia', value:'colombia'},
        {name: 'Peru', value:'peru'},
        {name: 'España', value:'spain'},
        {name: 'Brazil', value:'brazil'},
      ],
      selectedCountry: 'argentina',
      loading:true,
    }
  },
  components: {
    // Artist: Artist
    Artist,
    Spinner
  },
  methods: {
    refreshArtists (){
      const self = this
      this.loading = true
      this.artists = []
      getArtists(this.selectedCountry)
      .then (function (artists){
        self.loading = false
        self.artists = artists
      })
    }
  },
  mounted () {
    this.refreshArtists()
  },
  watch: {
    selectedCountry() {
      this.refreshArtists()
    }
  }
}
</script>

<style lang="stylus">
*  
  margin 0
  padding 0

#app 
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color #2c3e50
  margin-top 60px

h1, h2 
  font-weight normal

ul
  margin-right 0

artist
  margin 0
</style>

