<template>
  <div class="container">
    <div>
      <NuxtLink to="/"> Homepage </NuxtLink>
      <h1 class="title">
        Upload
      </h1>
      <div>
        <div class="form-section">
          <label for="name">Name</label>
          <div class="error" v-if="errors.name">{{errors.name}}</div>
          <input type="text" id="name" v-model="formData.name" @keypress="errors.name = null" />
        </div>
        <div class="form-section">
          <label for="fileUploadZone">Upload</label>
          <div class="error" v-if="errors.file">{{errors.file}}</div>
          <div class="dropzone" id="fileUploadZone"></div>
        </div>
        <div class="form-section">
          <label for="desc">Description</label>
          <textarea id="desc" rows="4" cols="33" v-model="formData.description"></textarea>
        </div>
        <div class="form-section">
          <button @click="submit">Upload</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Dropzone from "dropzone";
import "dropzone/dist/dropzone.css";

export default {
  data() {
    return {
      errors: {
        name: null,
        file: null,
      },
      formData: {
        name: '',
        description: '',
        imageUrl: '',
        thumbnailUrl: '',
      }
    };
  },
  created() {
    setTimeout(() => {
      let myDropzone = new Dropzone("#fileUploadZone", { 
        url: `${this.$config.apiBaseURL}/api/upload`,
        maxFiles: 1,
        acceptedFiles: 'image/*',
      });
      myDropzone.on("complete", file => {
        const { response } = file.xhr;
        const data = JSON.parse(response);
        console.log("A file has been added", data);
        this.formData.imageUrl = data.imageUrl;
        this.formData.thumbnailUrl = data.thumbnailUrl;
        this.errors.file = null;
      });
    }, 500);
  },
  methods: {
    async submit() {
      const data = { // JSON.parse(JSON.stringify(this.formData));
        name: this.formData.name.trim().replace(/\s+/g, ' '),
        description: this.formData.description.trim(),
        imageUrl: this.formData.imageUrl,
        thumbnailUrl: this.formData.thumbnailUrl,
      };

      let errorCount = 0;

      if(!data.name.length) {
        this.errors.name = 'You must specify a name';
        errorCount++;
      }

      if(!data.imageUrl) {
        this.errors.file = 'Please upload a file';
        errorCount++;
      }

      if(!errorCount){
        const url = `${this.$config.apiBaseURL}/api/create`;
        const resData = await this.$axios.$post(url, data);
        if(resData.id){
          document.location = `/pic/${resData.id}`;
        }
      }
    }
  }
}
</script>


<style scoped lang="scss">

.form-section {
  > * {
    display: block;
    margin-bottom: 0.5em;
    margin-inline: auto;
  }
}

.error {
  color: red;
}

img {
  max-width: 90vw;
  max-height: 70vh;
  object-fit: contain;
  display: block;
  margin: 1em auto;
}

</style>
