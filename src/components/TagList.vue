<template>
  <div class="TagList">
    <div class="sidebar">
      <h3>Popular Tags</h3>
      <ul>
        <a v-for="(tag, index) in tags" :key="index" v-on:click="$emit('set-tag', tag)">
          <li class="tag-pill tag-default">{{tag}}</li>
        </a>
      </ul>
    </div>
  </div>
</template>

<script>
export default {
  name: "TagList",
  props: {
    tags: Array
  }
};
</script>

<style>
ul > a {
  cursor: pointer;
}
</style>
