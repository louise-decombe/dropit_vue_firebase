<template>
  <div id="app">
    <vue-dropzone ref="imgDropzone" :options="dropzoneOptions" @vdropzone-complete="afterComplete">

      <div v-if="images.lentgh > 0">
        
<div v-for="image in images" :key="image.src"> </div>
<image :src="image"  
         </div>



    </vue-dropzone>

</template>

<script>
import firebase from 'firebase';
import vue2Dropzone from 'vue2-dropzone';
import "vue2-dropzone/dist/vue2Dropzone.min.css";
let uuid = require("uuid");

export default {
  name: 'App',
  components: {
    vueDropzone: vue2Dropzone;
  },
  data(){
    return{
      images: [],
      dropzoneOptions: {
        url:"https://httpbin.org/post",
        thumbnailWidth: 250,
        thumbnailHeight: 250,
        addRemoveLinks: true,
        acceptedFiles: ".jpg, .jpeg, .png"
      }
    }
  },

methods: {
  async afterComplete(file) {
    try {
      
      const imageName = uuid.v1();
      const metadata = {
        contentType: "image/png"
      }

      const storageRef = firebase.storage().ref; 
      const imageref = storageRef.child('image/${imageName}.png')

      await imageref.put(file, metadata);

      const downloadUrl = await imageref.getDownloadUrl()
      this.$refs.imgDropzone.removeFile(file);

    } catch (error) {
      console.log(error);
    }
  }
},

}
</script>

<style>

</style>
