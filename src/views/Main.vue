<template>
  <div>
      <div class="d-flex flex-wrap" v-if="nowPlaying.length">
          <movie-text text="NowPlaying"></movie-text>
          <movie-lists :movieList="nowPlaying"></movie-lists>
          <movie-text text="Popular"></movie-text>
          <movie-lists :movieList="popular"></movie-lists>
          <movie-text text="Comming Soon"></movie-text>
          <movie-lists :movieList="upComing"></movie-lists>
      </div>
  </div>
</template>

<script>
import MovieText from "../components/MovieText"
import MovieLists from "../components/MovieLists";
import { movieApi } from "../utils/axios";
import {mapMutations} from "vuex"

export default {
  components: {
    MovieLists,
    MovieText,
  },
  data(){
      return {
          nowPlaying:[],
          popular:[],
          upComing:[]
      }
  },
  created() {
      this.SET_LOADING(true)
  },
  async mounted() {
    try {
      const { nowPlaying, popular, upComing } = movieApi;
      const nowPlayingResult = await nowPlaying();
      const popularResult = await popular();
      const upComingResult = await upComing();

      this.SET_LOADING(false)

      this.nowPlaying = nowPlayingResult.data.results;
      this.popular = popularResult.data.results;
      this.upComing = upComingResult.data.results;

      //   const data = await movieApi.nowPlaying();
      //   console.log(data)
    } catch (error) {
      console.log(error);
    }
  },
  methods:{
      ...mapMutations(['SET_LOADING'])
  }
};
</script>

<style>
</style>
