<template>
  <base-dialog v-if="inputIsInvalid" title="Invalid Input" @close="confirmError">
    <template #default>
      <p>At least one input value is invalid.</p>
      <p>Please check all input and make sure you enter at least a few characters</p>
    </template>
    <template #actions>
      <base-button @click="confirmError">Okay</base-button>
    </template>
  </base-dialog>
  <base-card>
      <form @submit.prevent="submitData">
          <div class="form-control">
              <label for="title">Title</label>
              <input id="title" name="title" type="text" ref="titleInput">    
          </div>
          <div class="form-control">
              <label for="description">Description</label>
              <textarea id="description" name="description" rows="3" ref="descInput"></textarea>   
          </div>
          <div class="form-control">
              <label for="link">Link</label>
              <input id="link" name="link" type="url" ref="linkInput">    
          </div>
          <div>
              <base-button type="submit">Add Resource <loading-spinner v-if="isPostingToFirebase"></loading-spinner></base-button>
              <p v-if="isPostingCompleted">Successfuly Uploaded!</p>  
          </div>
      </form>
  </base-card>
</template>

<script>
import axios from "axios";

export default ({
  data() {
    return {
      inputIsInvalid: false,
      isPostingToFirebase: false,
      isPostingCompleted:false,
    }
  },
  methods: {
    submitData() {
      this.isPostingToFirebase = true;
      this.isPostingCompleted = false;
      const enteredTitle = this.$refs.titleInput.value;
      const enteredDescription = this.$refs.descInput.value;
      const enteredUrl = this.$refs.linkInput.value;
      // Form validation
      if (enteredTitle.trim() === '' || enteredDescription.trim() === '' || enteredUrl.trim() === '') {
        this.inputIsInvalid = true;
        this.isPostingToFirebase = false;
        return;
      }
      // Axios POST to Firebase
      axios({
        method: 'post',
        url: 'https://learning-resources-app-a2444-default-rtdb.asia-southeast1.firebasedatabase.app/learning-resources.json',
        data: {
          title: enteredTitle,
          description: enteredDescription,
          url: enteredUrl
        }
      }).catch(error => {
        console.log(error.message)
      })
      this.isPostingToFirebase = false;
      this.isPostingCompleted = true;
    },
    confirmError() {
      this.inputIsInvalid = false;
    }
  }
})
</script>


<style scoped>
label {
  font-weight: bold;
  display: block;
  margin-bottom: 0.5rem;
  color: white;
}

input,
textarea {
  display: block;
  width: 100%;
  font: inherit;
  padding: 0.15rem;
  border: 1px solid #ccc;
}

input:focus,
textarea:focus {
  outline: none;
  border-color: #5394be;
  background-color: #779bb4;
}
form {
  width: 100%;
}
.form-control {
  margin: 1rem 0;
}

p {
  color: white
}
</style>