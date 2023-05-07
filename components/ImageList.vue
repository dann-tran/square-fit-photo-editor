<script lang="ts">
export default {
  props: {
    files: {type: Array as PropType<File[]>, required: true}
  },
  methods: {
    generateUrl(file: File) {
      let fileSrc = URL.createObjectURL(file);
      setTimeout(() => {
        URL.revokeObjectURL(fileSrc);
      }, 1000);
      return fileSrc;
    }
  }
}
</script>

<template>
  <div class="preview-container" v-if="files.length">
    <div v-for="(file, idx) in files" :key="file.name" class="preview-card">
      <div>
        <img class="preview-img" :src="generateUrl(file)" />
        <p>
          {{ file.name }}
        </p>
      </div>
      <div>
        <button
          class="ml-2"
          type="button"
          @click="files.splice(idx)"
          title="Remove file"
        >
          <b>×</b>
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.preview-container {
  display: flex;
  margin-top: 2rem;
}

.preview-card {
  display: flex;
  border: 1px solid #a2a2a2;
  padding: 5px;
  margin-left: 5px;
}

.preview-img {
  border-radius: 5px;
  border: 1px solid #a2a2a2;
  background-color: #a2a2a2;
}
</style>