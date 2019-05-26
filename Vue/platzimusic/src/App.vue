<template lang="pug">
    #app
      img(src='https://edenverdugo.github.io/PlatziCursos/Vue/platzimusic/dist/logo.png')
      h1 PlatziMusic
      select(v-model="selectedCountry")
        option(v-for="country in countries" :value="country.value") {{country.name}}
      spinner(v-show="loading")
      ul 
        artist(v-for="artist in artists" :artist="artist" :key="artist.mbid") 

      
</template>

<script>
import getArtists from './api'
import artist from './components/artist'
import spinner from './components/Spinner'

export default {
  name: 'app',
  data () {
    return {
      artists: [],
      countries:[
        { name: 'Mexico', value:'mexico'},
        { name: 'Colombia', value:'colombia'},
        { name: 'España', value:'spain'},
        { name: 'Canada', value:'canada'}
      ],
      selectedCountry: 'mexico',
      loading:true
    }
  },
  mounted: function(){  
    this.refreshArtists()  
  },
  methods: {
    refreshArtists(){
      const self = this

      this.loading = true
      this.artists = []

      getArtists(this.selectedCountry)
      .then((artists)=> {
        self.artists = artists
        console.log(self.artists)
      })      
      .catch((error)=>{
        console.log(error)
      })
      .finally(()=>{
        this.loading = false
      })
    }
  },
  watch: {
    selectedCountry: function(){
      this.refreshArtists()
    }
  },
  components: {
    artist,
    spinner
  }
}
</script>

<style lang="stylus">
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
  list-style-type none
  padding 0

li
  display inline-block
  margin 0 10px

a
  color #42b983
</style>
