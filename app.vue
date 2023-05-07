<script lang="ts">
import DropFileInput from "./components/DropFileInput.vue"

export default {
  components: {
    DropFileInput
  },
  setup() {
    const fileinput = ref<InstanceType<typeof DropFileInput>>();
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
  <div>
    <DropFileInput ref="fileinput"/>
    <button @click="exportImg">Export</button>
  </div>
</template>
