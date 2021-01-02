<template>
  <div>
    <ul class="list-group">
      <VideoListItem
        v-for="video in fooItems.slice(1)"
        v-bind:key="video.etag"
        v-bind:video="video"
        v-on:fooCustomName="onVideoSelection"
      />
    </ul>
  </div>
</template>

<script>
import VideoListItem from './VideoListItem';

export default {
  name: 'VideoList',
  components: {
    VideoListItem
  },
  props: ['fooItems'], // receiving this from parent
  methods: {
    onVideoSelection(video) {
      // in the second argument, pass along the video that was selected in the child component
      this.$emit('barOfFoo', video);
    }
  }
};
</script>

<style lang="scss" scoped>
div {
  overflow: hidden;
  height: calc(100vh - 139px);
}
.list-group {
  height: 100%;
  width: 100%;
  overflow: auto;
  margin: 0;
  padding: 0 10px 0 0;
  border-radius: 0 !important;
}
</style>
