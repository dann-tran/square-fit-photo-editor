<script lang="ts">
import ImageInput from "./components/ImageInput.vue"

export default {
  components: {
    ImageInput
  },
  setup() {
    const fileinput = ref<InstanceType<typeof ImageInput>>();
    return {
      fileinput
    }
  },
  methods: {
    exportImg(e: MouseEvent) {
      e.preventDefault();

      const canvas = document.createElement("canvas");
      const ctx = canvas.getContext("2d");

      if (!ctx) {
        // throw error
        return;
      }
      if (!this.fileinput) {
        // throw error
        return;
      }

      for (const file of this.fileinput.files) {
        const reader = new FileReader();
        reader.onload = function() {
          let img = new Image();
          img.onload = function() {
              const imgDim = Math.max(img.width, img.height);
              canvas.width = imgDim;
              canvas.height = imgDim;
              ctx.drawImage(img, (imgDim - img.width) / 2, (imgDim - img.height) / 2, img.width, img.height);
              
              const url = canvas.toDataURL("image/jpeg");
              const a = document.createElement("a");
              a.href = url;
              a.download = file.name;
              a.click();
            }
          img.src = String(reader.result);
        }
        reader.readAsDataURL(file);
      }
    }
  }
}
</script>

<template>
  <div id="container">
    <h1>Square-fit photo editor</h1>
    <ImageInput ref="fileinput"/>
    <button v-if="fileinput && fileinput.files.length" @click="exportImg">Export</button>
  </div>
</template>

<style scoped>
#container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 30px;
}

button {
  width: min(5rem, 100%);
  height: 2rem;
}
</style>