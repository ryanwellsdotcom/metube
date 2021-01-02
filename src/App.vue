<template>
  <div class="container">
    <div class="row">
      <div class="col-8 header">
        <h1>metube</h1>
        <SearchBar v-on:TermChange="onSearch"></SearchBar>
      </div>
    </div>

    <div class="row">
      <div class="col-8">
        <VideoDetail :video="selectedVideo" />
      </div>
      <div class="col-4">
        <VideoList v-on:barOfFoo="onVideoSelect" v-bind:fooItems="videos" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import SearchBar from './components/SearchBar';
import VideoList from './components/VideoList';
import VideoDetail from './components/VideoDetail';

export default {
  name: 'App',
  components: {
    SearchBar,
    VideoList,
    VideoDetail
  },
  data() {
    return {
      videos: [], // binding to custom prop to pass
      selectedVideo: null,
      apiSecret: process.env.VUE_APP_API_KEY
    };
  },
  methods: {
    onVideoSelect(video) {
      this.selectedVideo = video;
    },
    onSearch(searchTerm) {
      axios
        .get('https://www.googleapis.com/youtube/v3/search', {
          params: {
            key: this.apiSecret,
            type: 'video',
            part: 'snippet',
            maxResults: 50,
            safeSearch: 'strict',
            regionCode: 'US',
            videoEmbeddable: true,
            q: searchTerm
          }
        })
        .then(response => {
          this.videos = response.data.items;
          this.selectedVideo = response.data.items[0];
        });
      // console.log(searchTerm);
    }
  }
};
</script>

<style lang="scss" scoped>
.header {
  display: flex;
  justify-content: flex-start;
  margin: 45px 0;
}
h1 {
  margin: -8px 30px 0 0;
  padding: 0;
}
.container {
  max-width: 95%;
}
</style>
