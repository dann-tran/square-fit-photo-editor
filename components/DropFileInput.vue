<script lang="ts">
import ImageList from "./ImageList.vue"

export default {
  components: {
    ImageList
  },
  expose: ["files"],
  data() {
    return {
      isDragging: false,
      files: [] as File[],
    }
  },
  methods: {
    handleSetDragging(value: boolean) {
      return (e: Event) => {
        e.preventDefault();
        this.isDragging = value;
      }
    },
    uploadFile(e: Event) {
      e.preventDefault();

      // validate image files
      const files = (this.$refs.file as HTMLInputElement).files
      if (!files) {
        // raise error
        return;
      }
      
      console.log(files);
      this.files = [...files];
    },
    handleDropFiles(e: DragEvent) {
      e.preventDefault();
      
      // validate image files
      const files = (this.$refs.file as HTMLInputElement).files
      if (!files) {
        // raise error
        return;
      }
      
      this.files = [...files];

      this.isDragging = false;
    }
  }
}
</script>

<template>
  <div>
    <div
      id="dropzone"
      @dragover="handleSetDragging(true)"
      @dragleave="handleSetDragging(false)"
      @drop="handleDropFiles"
      @click="($refs.file as HTMLInputElement).click"
    >
      <input
        type="file"
        multiple
        name="file"
        id="file-input"
        @change="uploadFile"
        ref="file"
        accept=".pdf,.jpg,.jpeg,.png"
      />
      <label for="file-input" id="file-input-label">
        <div v-if="isDragging">Release to drop files here.</div>
        <div v-else>Drop files here or click to upload</div>
      </label>
    </div>
    <ImageList :files="files"/>
  </div>
</template>

<style scoped>
#dropzone {
  padding: 4rem;
  background: #f7fafc;
  border: 1px solid #e2e8f0;
  cursor: pointer;
}

#file-input {
  opacity: 0;
  overflow: hidden;
  position: absolute;
  width: 1px;
  height: 1px;
}

#file-input-label {
  cursor: pointer;
}
</style>