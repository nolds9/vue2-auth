<style>
  .exclamations-viewer,
  .add-form-container {
    margin-top: 20px;
  }
</style>

<template>
  <div class="container">
    <div class="row exclamations-viewer">
      <div class="col-md-4">
        <Exclamation-List
          :user='user'
          title='All Exclamations'
          :exclamations='exclamations'
          :onRemove='handleExclamationRemoved' >
        </Exclamation-List>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import ExclamationList from './exclamation_list.vue';

  export default {
    name: 'ExclamationsViewer',
    data: () => ({
      user: {
        scopes: [],
      },
      exclamations: [],
    }),
    beforeMount() {
      axios.all([
        axios.get('/api/me'),
        axios.get('/api/exclamations'),
      ]).then(([{ data: meData }, { data: exclamationData }]) => {
        this.user = meData.user;
        this.exclamations = exclamationData.exclamations;
      });
    },
    components: {
      ExclamationList,
    },
    methods: {
      handleExclamationRemoved(id) {
        axios.delete(`/api/exclamations/${id}`)
          .then(() => {
            this.exclamations = this.exclamations.filter(e => e.id !== id)
          });
      },
    },
  };
</script>
