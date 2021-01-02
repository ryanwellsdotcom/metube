<template>
  <form @keydown.enter="onInput">
    <label for="search" class="form-label visually-hidden"
      >Search YouTube</label
    >
    <input id="search" class="form-control" type="text" placeholder="Search" />
  </form>
</template>

<script>
export default {
  name: 'SearchBar',
  methods: {
    onInput: function(event) {
      event.preventDefault();
      if (event.target.value.trim() !== '') {
        this.$emit('termChange', event.target.value.trim());
      }
    }
  }
};
</script>

<style lang="scss" scoped>
form {
  flex-grow: 1;
}
</style>
