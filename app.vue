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
  <div id="app-container">
    <h1>Square-fit photo editor</h1>
    <ImageInput ref="fileinput" class="image-input"/>
    <button v-if="fileinput && fileinput.files.length" @click="exportImg">Export</button>
  </div>
</template>

<style scoped>
#app-container {
  width: min(100vw, 30rem);
  height: 98vh;
  background-color: rgba(255, 255, 255, 0.05);
  padding: 0 1.5rem;
}

.image-input {
  margin-bottom: 1.5rem;
}

button {
  width: min(5rem, 100%);
  height: 2rem;
  background-color: rgba(255, 255, 255, 0.05);
  border: 1.5px solid rgba(255, 255, 255, 0.25);
  border-radius: 4.5px;
  display: block;
  margin-left: auto;
  margin-right: auto;
}
</style>

<style global>
* {
  font-family: Arial, Helvetica, sans-serif;
  color: rgba(255, 255, 255, 0.95);
}

#__nuxt {
  display: flex;
  justify-content: center;
}

body {
  background-color: #121212;
}
</style>