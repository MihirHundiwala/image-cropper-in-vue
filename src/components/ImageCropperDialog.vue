<template>
  <div class="crop-image-dialog">
    <v-dialog v-model="showCropper" max-width="500" persistent>
      <v-card class="pt-6 pb-3">
        <v-card-text class="pb-3">
          <vue-cropper
            ref="cropper"
            class="image-container"
            :aspect-ratio="1 / 1"
            :guides="true"
            :background="false"
            :view-mode="3"
            drag-mode="move"
            :src="chosenImage"
            alt="Image not available"
          >
          </vue-cropper>
        </v-card-text>
        <v-card-actions class="py-0 mx-10">
          <v-btn
            @click="resetCropper"
            text color="red"
          > Cancel </v-btn>
          <v-spacer></v-spacer>
          <v-btn 
            @click="cropChosenImage"
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
        chosenImage: {
            default: null,
        },
    },
    data () {
        return {
            showCropper: false,
            imageFileType: null,
        }
    },
    methods: {
        async initCropper(imageFileType) {
            this.showCropper = true;
            this.imageFileType = imageFileType;
            await new Promise(resolve => setTimeout(resolve, 50));
            this.$refs.cropper.replace(this.chosenImage);
        },

        async resetCropper(){
          this.$emit('onReset');
          this.showCropper = false;
        },

        async cropChosenImage(){
            this.$emit('onCrop', this.$refs.cropper.getCroppedCanvas().toDataURL(this.imageFileType));
            this.resetCropper();
        },
    },
}
</script>

<style>
.image-container {
  max-width: 450px;
}

.image-container img {
  /* This is important */
  width: 100%;
}
</style>