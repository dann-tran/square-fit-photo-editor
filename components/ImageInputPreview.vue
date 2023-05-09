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
  width: 100%;
}

.preview-card {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  border-top: 1px solid rgba(255, 255, 255, 0.25);
  padding: 0.75rem 0;
}

.preview-card:last-of-type {
  border-bottom: 1px solid rgba(255, 255, 255, 0.25);
}

img {
  height: 4rem;
  width: 4rem;
  object-fit: contain;
  background-color: black;
}

button {
  background-color: transparent;
  border: none !important;
  margin: 0.5rem;
  font-size: x-large;
}
</style>