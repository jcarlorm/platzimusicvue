<template lang="pug">
#app
  img(src='https://jcarlorm.github.io/platzimusicvue/src/assets/logo.png')
  h1 PlatziMusic
  select(v-model="selectedCountry")
    option( v-for="country in countries" v-bind:value="country.value" v-text="country.name") 
  spinner(v-show="loading")
  ul
    artist(v-for="artist in artists" v-bind:artist="artist" v-bind:key="artist.mbid") 

</template>

<script>


import Artist from './components/Artist.vue'
import getArtists from './api'
import Spinner  from './components/Spinner.vue'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries: [
        {name: 'Argentina' , value: 'argentina'},
        {name: 'Costa Rica' , value: 'Costa Rica'},
        {name: 'Colombia' , value: 'Colombia'},
      ],
      selectedCountry: 'argentina',
      loading: true
    }
  },
  components: {
      Artist,
      Spinner
  },
  methods: {
    refreshArtist(){
      this.loading = true
      this.artists = []
      const self = this
      getArtists( this.selectedCountry )
      .then( function(artists) {
        self.artists = artists
        self.loading = false
      })
    }
  },
  mounted: function() {
    this.refreshArtist()
  },
  watch: {
    selectedCountry() {
      this.refreshArtist()
    }
  }

}
</script>

<style lang="stylus">
#app
  font-family 'Avenir', Helvetica, Arial, sans-serif
  -webkit-font-smoothing antialiased
  -moz-osx-font-smoothing grayscale
  text-align center
  color black
  margin-top 60px

h1, h2
  font-weight normal

ul
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
