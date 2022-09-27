<template>
    <div class="d-flex flex-column justify-center align-center fill-height">
      <h3>Image Cropper using Vuetify and <a href="https://www.npmjs.com/package/vue-cropperjs" target="_blank">vue-cropper-js</a></h3>
      
      <v-file-input 
        v-show="false" ref="imageField" 
        @change="uploadImage" accept="image/*"
      ></v-file-input>

      <div>
        <v-avatar size="350px" class="mt-5" style="border: 2px solid black;">
          <v-img :src="avatarImage"></v-img>
        </v-avatar>
      </div>

      <v-btn class="mt-5"
        @click="$refs.imageField.click()"
      > Upload </v-btn>

      <image-cropper-dialog
        ref="cropperDialog"
        :uploadedImage="uploadedImage"
        @onCrop="(croppedImage) => {
          avatarImage = croppedImage;
        }"
      />
    </div>
</template>

<script>
import ImageCropperDialog from './ImageCropperDialog.vue';
export default {
  name: 'HomePage',
  components: {
    ImageCropperDialog,
  },
  data() {
    return {
      avatarImage: require("../assets/default.jpg"),
      uploadedImage: null,
    }
  },
  methods: {
    async uploadImage(event) {
      if (!event) return;
      var file = event.target.files[0];
      this.uploadedImage = await this.toBase64(file);
      this.$refs.cropperDialog.initCropper(file.type);
    },

    async toBase64(file) {
      return new Promise((resolve, reject) => {
        const reader = new FileReader();
        reader.readAsDataURL(file);
        reader.onload = () => resolve(reader.result);
        reader.onerror = error => reject(error);
      });
    },
  }
}
</script>