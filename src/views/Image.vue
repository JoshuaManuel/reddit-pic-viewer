<template>
  <div>
    <img :src="imageUrl" alt="image">
  </div>
</template>

<script>
import { ipcRenderer } from "electron";

function htmlDecode(input)
{
  var doc = new DOMParser().parseFromString(input, "text/html");
  return doc.documentElement.textContent;
}

export default {
    created() {
        ipcRenderer.on('image', (event, arg) => {
            this.imageUrl = htmlDecode(arg)
        })
    },
    data() {
        return {
            imageUrl: ''
        }
    }
}
</script>

<style>
img {
    max-width:100%;
}
</style>
