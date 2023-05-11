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
  computed: {
    dropzoneText() {
      return this.isDragging 
        ? "Release to drop images here" 
        : "Drop images here or click to import";
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
  <div id="image-input-container">
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
      <div>{{ dropzoneText }}</div>
    </div>
    <ImageInputPreview 
      :files="files"
      @remove-file="removeFile"
    />
  </div>
</template>

<style scoped>
#image-input-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 1rem;
  width: 100%;
}

#dropzone {
  padding: 3rem 0;
  width: 100%;
  text-align: center;
  background-color: rgba(255, 255, 255, 0.05);
  border-radius: 10px;
  border-style: solid;
  border-color: rgba(255, 255, 255, 0.25);
  cursor: pointer;
}

#file-input {
  display: none;
}
</style>