<template>
  <div class="hello">
     <ul class="list-group list-group-flush">
      <li class="list-group-item" v-for="post in posts" :key="post.data.id" @click="openImage(post.data.preview.images[0].source.url)">
          <img :src="post.data.thumbnail" alt="thumb" class="thumbnail">
          {{post.data.title}}
      </li>
    </ul>
  </div>
</template>

<script>
const axios = require("axios")
const {ipcRenderer} = require("electron")

export default {
  name: 'HelloWorld',
  data() {
      return {
          posts: []
      }
  },
  created() {
      axios.get("https://reddit.com/r/pics.json")
      .then(response => {
          this.posts = response.data.data.children
      })
      .check(error => {
          console.log(error)
      })
  },
  methods: {
    openImage(image) {
        ipcRenderer.send("toggle-image", image)
      }
  }
}



</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}

.thumbnail {
    width: 60px;
    height:60px;
    border-radius: 30px;
    margin-right: 16px;
}

.list-group-item {
    cursor: pointer
}

.list-group-item:hover {
    background-color: #eee;
}

.list-group {
  display: flex;
  flex-direction: column;

  // No need to set list-style: none; since .list-group-item is block level
  padding-left: 0; // reset padding because ul and ol
  margin-bottom: 0;
}

.list-group-item {
  position: relative;
  display: block;
  padding: $list-group-item-padding-y $list-group-item-padding-x;
  // Place the border on the list items and negative margin up for better styling
  margin-bottom: -$list-group-border-width;
  background-color: $list-group-bg;
  border: $list-group-border-width solid $list-group-border-color;

  &:first-child {
    @include border-top-radius($list-group-border-radius);
  }

  &:last-child {
    margin-bottom: 0;
    @include border-bottom-radius($list-group-border-radius);
  }

  &.disabled,
  &:disabled {
    color: $list-group-disabled-color;
    pointer-events: none;
    background-color: $list-group-disabled-bg;
  }

  // Include both here for `<a>`s and `<button>`s
  &.active {
    z-index: 2; // Place active items above their siblings for proper border styling
    color: $list-group-active-color;
    background-color: $list-group-active-bg;
    border-color: $list-group-active-border-color;
  }
}

.list-group-flush {
  .list-group-item {
    border-right: 0;
    border-left: 0;
    @include border-radius(0);

    &:last-child {
      margin-bottom: -$list-group-border-width;
    }
  }

  &:first-child {
    .list-group-item:first-child {
      border-top: 0;
    }
  }

  &:last-child {
    .list-group-item:last-child {
      margin-bottom: 0;
      border-bottom: 0;
    }
  }
}
</style>
