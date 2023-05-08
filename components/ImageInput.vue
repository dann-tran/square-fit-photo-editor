<script lang="ts">
import ImageInputPreview from "./ImageInputPreview.vue"

const IMAGE_MIME_TYPES = [
  "image/png",
  "image/jpeg"
]

export default {
  components: {
    ImageInputPreview
  },
  expose: ["files"],
  data() {
    return {
      isDragging: false,
      files: [] as File[],
      invalidFiles: [] as File[],
    }
  },
  methods: {
    handleDragOver(e: DragEvent) {
      e.preventDefault();
      this.isDragging = true;
    },
    handleDragLeave(e: DragEvent) {
      e.preventDefault();
      this.isDragging = false;
    },
    filterFiles(files: FileList | null | undefined) {
      if (!files) {
        return []
      }
      const outputFiles: File[] = [];
      for (const file of files) {
        if (IMAGE_MIME_TYPES.includes(file.type)) {
          outputFiles.push(file);
        } else {
          this.invalidFiles.push(file);
        }
      }
      return outputFiles;
    },
    removeFile(idx: number) {
      this.files.splice(idx, 1);
    },
    handleInputChange(e: Event) {
      e.preventDefault();
      const files = this.filterFiles((this.$refs.file as HTMLInputElement).files);
      this.files.push(...files);
    },
    handleDropFiles(e: DragEvent) {
      e.preventDefault();

      const files = this.filterFiles(e.dataTransfer?.files);
      this.files.push(...files);

      this.isDragging = false;
    }
  }
}
</script>

<template>
  <div id="container">
    <div
      id="dropzone"
      @dragover="handleDragOver"
      @dragleave="handleDragLeave"
      @drop="handleDropFiles"
      @click="($refs.file as HTMLInputElement).click"
    >
      <input
        type="file"
        multiple
        name="file"
        id="file-input"
        @change="handleInputChange"
        ref="file"
        accept=".jpg,.jpeg,.png"
      />
      <label for="file-input" id="file-input-label">
        <div v-if="isDragging">Release to drop files here.</div>
        <div v-else>Drop files here or click to import</div>
      </label>
    </div>
    <ImageInputPreview 
      :files="files"
      @remove-file="removeFile"
    />
  </div>
</template>

<style scoped>
#container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 30px;
}

#dropzone {
  padding: 4rem;
  margin: 5px;
  width: 30rem;
  text-align: center;
  background: #f7f7f7;
  border-radius: 10px;
  border-style: solid;
  border-color: #e5e5e5;
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