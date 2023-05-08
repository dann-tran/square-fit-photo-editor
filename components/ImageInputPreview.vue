<script lang="ts">
export default {
  props: {
    files: {type: Array as PropType<File[]>, required: true}
  },
  computed: {
    fileUrls() {
      return this.files.map(file => URL.createObjectURL(file));
    }
  },
  unmounted() {
    this.fileUrls.forEach(url => URL.revokeObjectURL(url));
  }
}
</script>

<template>
  <div id="preview-container" v-if="files.length">
    <div v-for="(file, idx) in files" :key="file.name" class="preview-card">
      <img :src="fileUrls[idx]" />
      <p>
        {{ file.name }}
      </p>
      <button
        @click="$emit('removeFile', idx)"
      >
        <b>×</b>
      </button>
    </div>
  </div>
</template>

<style scoped>
#preview-container {
  /* margin: 0 auto; */
  display: grid;
  gap: 1rem;
}

@media (min-width: 600px) {
  #preview-container {
    grid-template-columns: repeat(2, 1fr);
  }
}

.preview-card {
  display: flex;
  justify-content: space-around;
  align-items: center;
  gap: 2rem;
  height: 120px;
  width: 100%;
  border-style: solid;
  border-color: #e2e2e2;
  border-radius: 5px;
}

img {
  height: 100px;
  width: 100px;
  object-fit: contain;
  background-color: black;
  margin: 0.5rem;
}

button {
  color: rgb(171, 171, 171);
  background-color: transparent;
  border: none !important;
  margin: 0.5rem;
}
</style>