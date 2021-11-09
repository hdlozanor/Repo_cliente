<template>
  <div v-if="currentFile" class="edit-form">
    <h4>File</h4>
    <form>
      <div class="form-group">
        <label for="title">Title</label>
        <input type="text" class="form-control" id="title"
          v-model="currentFile.title"
        />
      </div>
      <div class="form-group">
        <label for="description">Description</label>
        <input type="text" class="form-control" id="description"
          v-model="currentFile.description"
        />
      </div>
      <div class="form-group">
        <label for="language">Language</label>
        <input type="text" class="form-control" id="language"
          v-model="currentFile.language"
        />
      </div>

    </form>


    <button class="badge badge-danger mr-2"
      @click="deleteFile"
    >
      Delete
    </button>

    <button type="submit" class="badge badge-success"
      @click="updateFile"
    >
      Update
    </button>
    <p>{{ message }}</p>
  </div>

  <div v-else>
    <br />
    <p>Please click on a File...</p>
  </div>
</template>

<script>
import FileDataService from "../services/FileDataService";

export default {
  name: "file",
  data() {
    return {
      currentFile: null,
      message: ''
    };
  },
  methods: {
    getFile(id) {
      FileDataService.get(id)
        .then(response => {
          this.currentFile = response.data;
          console.log(response.data);
        })
        .catch(e => {
          console.log(e);
        });
    },


    updateFile() {
      var data = {
        title: this.currentFile.title,
        description: this.currentFile.description,
        language: this.currentFile.language
      };
      console.log(data)
      FileDataService.update(this.currentFile[0].id, data)
        .then(response => {
          console.log(response.data);
          this.message = 'The file was updated successfully!';
        })
        .catch(e => {
          console.log(e);
        });
    },

    deleteFile() {
      FileDataService.delete(this.currentFile.id)
        .then(response => {
          console.log(response.data);
          this.$router.push({ name: "files" });
        })
        .catch(e => {
          console.log(e);
        });
    }
  },
  mounted() {
    this.message = '';
    this.getFile(this.$route.params.id);
  }
};
</script>

<style>
.edit-form {
  max-width: 300px;
  margin: auto;
}
</style>
