<template lang="pug">
   #app
      img(src='https://darwin325.github.io/platzimusic/dist/logo.png')
      h1 PlatziMusic
      select(v-model="selectedCountry")
         option(v-for="country in countries" :value="country.value") {{ country.name }}
      spinner(v-show="loading")
      ul
         artists(v-for="artist in artists" :artist="artist" :key="artist.mbid")
</template>

<script>
    import getArtists from './api'
    import Artists from "./components/Artists";
    import Spinner from "./components/Spinner";

    export default {
        name: 'app',
        components: {Spinner, Artists},
        data () {
            return {
                artists: [],
                countries : [
                    {name : 'Argentina', value : 'argentina'},
                    {name : 'Colombia', value : 'colombia'},
                    {name : 'España', value : 'spain'}
                ],

                selectedCountry : 'argentina',

                loading : true,
            }
        },

        methods : {

            refreshArtist : function () {
                const self = this;
                this.loading = true;
                this.artists = [];
                getArtists(this.selectedCountry)
                    .then(function (artists) {
                        self.loading = false;
                        self.artists = artists;
                    });
            }
        },

        mounted () {
            this.refreshArtist(this.selectedCountry);
        },

        watch : {
            selectedCountry : function () {
                this.refreshArtist(this.selectedCountry);
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
