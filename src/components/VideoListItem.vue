<template>
  <li class="list-group-item" v-on:click="onVideoSelect">
    <div class="me-2 img-wrapper">
      <img v-bind:src="thumbnailUrl" />
      <span>{{ videoDuration }}</span>
    </div>
    <div class="text">
      <p class="title" v-html="title"></p>
      <p class="channel" v-html="channel"></p>
    </div>
  </li>
</template>

<script>
import axios from 'axios';
export default {
  name: 'VideoListItem',
  props: {
    video: Object // using object for type validation
  },
  data() {
    return {
      videoDuration: '',
      apiSecret: process.env.VUE_APP_API_KEY
    };
  },
  created() {
    this.getVideoDuration();
  },
  updated() {
    this.getVideoDuration();
  },
  computed: {
    thumbnailUrl() {
      return this.video.snippet.thumbnails.medium.url;
    },
    title() {
      return this.video.snippet.title;
    },
    channel() {
      return this.video.snippet.channelTitle;
    }
  },
  methods: {
    onVideoSelect() {
      // emit even on click
      // first argument is a custom name. second argument is the item that was clicked.
      this.$emit('fooCustomName', this.video);
    },
    getVideoDuration() {
      axios
        .get('https://www.googleapis.com/youtube/v3/videos', {
          params: {
            key: this.apiSecret,
            part: 'contentDetails',
            id: this.video.id.videoId
          }
        })
        .then(response => {
          response.data.items.forEach(e => {
            this.formatDuration(e.contentDetails.duration);
          });
        });
    },
    formatDuration(duration) {
      const d = duration.substr(2);
      let formattedDur = '';
      let h = '';
      let m = '00';
      let s = '00';
      if (d.includes('H')) {
        h = d.split('H')[0];
        if (!d.includes('M')) {
          s = d.split('H')[1].split('S')[0];
        } else {
          m = d.split('H')[1].split('M')[0];
          if (d.includes('S')) {
            s = d
              .split('H')[1]
              .split('M')[1]
              .split('S')[0];
          }
        }
      } else if (d.includes('M')) {
        m = d.split('M')[0];
        if (d.includes('S')) {
          s = d.split('M')[1].split('S')[0];
        }
      }
      // ensure double digits for minutes
      if (parseInt(m) < 10 && h !== '' && m !== '00') {
        m = '0' + m;
      }
      // ensure double digits for seconds
      if (parseInt(s) < 10 && s !== '00') {
        s = '0' + s;
      }
      if (h !== '') {
        formattedDur += h + ':';
      }
      formattedDur += m + ':' + s;
      this.videoDuration = formattedDur;
    }
  }
};
</script>

<style lang="scss" scoped>
.list-group-item {
  display: flex;
  padding: 0;
  margin: 0 0 12px 0;
  border: none;
  min-height: 73px;
}
.img-wrapper {
  position: relative;
  min-width: 130px;
  height: 73px;
  img {
    position: absolute;
    width: 130px;
    max-height: 73px;
  }
  span {
    background: #000;
    color: #fff;
    padding: 1px 4px 2px 4px;
    font-size: 10px;
    font-weight: 600;
    position: absolute;
    right: 3px;
    bottom: 3px;
  }
}

li {
  background-color: #fff;
  margin: 5px 0;
  &:hover {
    cursor: pointer;
  }
  .text {
    font-size: 0.8125rem;
    line-height: 1.25;
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
    text-overflow: ellipsis;
    .title {
      margin-bottom: 4px;
      display: -webkit-box;
      -webkit-line-clamp: 2;
      -webkit-box-orient: vertical;
      overflow: hidden;
      font-weight: 500;
    }
    .channel {
      margin-bottom: 4px;
      color: #666;
      font-size: 0.75rem;
    }
  }
}
</style>
