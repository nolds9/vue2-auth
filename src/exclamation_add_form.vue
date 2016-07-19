<template lang="html">
  <form class="form-inline" v-on:submit.prevent='onFormSubmit'>
    <div class="form-group">
      <label for='exclamationText'>New Exclamation:</label>
      <textarea
        class='form-control'
        placeholder="Enter an exclamation"
        rows="2" cols="30"
        v-model='exclamationText' >
      </textarea>
    </div>
    <input type="submit" class="btn btn-success" value="Submit">
  </form>
</template>

<script>
  export default {
    data() {
      return {
        exclamationText: '',
      };
    },
    props: ['onAdd'],
    methods: {
      onFormSubmit() {
        this.onAdd(this.exclamationText);
        this.exclamationText = '';
      }
    }
  }
</script>
