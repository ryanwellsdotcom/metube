<template>
  <div class="embed-container" :class="{ hide: video == null }">
    <iframe
      :src="
        'https://www.youtube.com/embed/' +
          (video !== null ? video.id.videoId : null) +
          '?rel=0'
      "
      frameborder="0"
      allowfullscreen
    ></iframe>
  </div>
  <div>
    <h2 class="title" v-html="video !== null ? video.snippet.title : null"></h2>
    <p class="date">{{ videoDate }}</p>
    <p
      class="description"
      v-html="video !== null ? video.snippet.description : null"
    ></p>
  </div>
</template>

<script>
export default {
  name: 'VideoDetail',
  props: {
    video: Object
  },
  data() {
    return {
      monthNames: [
        'Jan',
        'Feb',
        'Mar',
        'Apr',
        'May',
        'Jun',
        'Jul',
        'Aug',
        'Sep',
        'Oct',
        'Nov',
        'Dec'
      ],
      videoDate: ''
    };
  },
  updated() {
    this.getDate();
  },
  methods: {
    getDate() {
      let newDate = new Date(this.video.snippet.publishTime.substr(0, 10));
      let month = this.monthNames[newDate.getMonth() - 1];
      let date = newDate.getDate();
      let year = newDate.getFullYear();
      this.videoDate = `${month} ${date}, ${year}`;
    }
  }
};
</script>

<style lang="scss" scoped>
.hide {
  display: none;
}
.embed-container {
  position: relative;
  padding-bottom: 56.25%;
  height: 0;
  overflow: hidden;
  max-width: 100%;
  background: #000;
  margin-bottom: 15px;
}
.embed-container iframe,
.embed-container object,
.embed-container embed {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}
.title {
  font-size: 20px !important;
}
.description {
  font-size: 14px;
  margin-bottom: 20px;
}
.date {
  font-size: 13px;
  color: #666;
  margin-bottom: 8px;
}
</style>
