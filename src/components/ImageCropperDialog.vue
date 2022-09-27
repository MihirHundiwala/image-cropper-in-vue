<template>
  <div class="crop-image-dialog">
    <v-dialog v-model="showCropper" max-width="500" persistent>
      <v-card class="pt-6 pb-3">
        <v-card-text class="pb-3">
          <vue-cropper
            ref="cropper"
            :containerStyle="{ 
              width: 'fit-content', 'max-width':'450px', 
              height: 'fit-content', 'max-height':'450px'
            }"
            :aspect-ratio="1 / 1"
            :guides="true"
            :background="false"
            :view-mode="3"
            drag-mode="move"
            :src="uploadedImage"
            alt="Image not available"
          >
          </vue-cropper>
        </v-card-text>
        <v-card-actions class="py-0 mx-10">
          <v-btn
            @click="showCropper = false"
            text color="red"
          > Cancel </v-btn>
          <v-spacer></v-spacer>
          <v-btn 
            @click="cropUploadedImage"
            text color="blue"
          > Crop </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
import VueCropper from 'vue-cropperjs';
import 'cropperjs/dist/cropper.css';

export default {
    name: "ImageCropperDialog",
    components: {
        VueCropper,
    },
    props: {
        uploadedImage: {
            default: null,
        },
    },
    data () {
        return {
            showCropper: true,
            imageFileType: null,
        }
    },
    methods: {
        async initCropper(imageFileType) {
            this.showCropper = true;
            this.imageFileType = imageFileType;
            await new Promise(resolve => setTimeout(resolve, 50));
            this.$refs.cropper.replace(this.uploadedImage);
        },
        async cropUploadedImage(){
            this.$emit('onCrop', this.$refs.cropper.getCroppedCanvas().toDataURL(this.imageFileType));
            this.showCropper = false;
        },
    },
}
</script>