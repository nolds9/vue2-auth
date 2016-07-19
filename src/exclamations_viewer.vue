<style>
  .exclamations-viewer,
  .add-form-container {
    margin-top: 20px;
  }
</style>

<template>
  <div class="container">
    <div class="row add-form-container" v-if="canAdd()">
      <div class="col-md-12">
        <Exclamation-Add-Form :onAdd="handleExclamationAdded"><Exclamation-Add-Form>
      </div>
    </div>
    <div class="row exclamations-viewer">
      <div class="col-md-4">
        <Exclamation-List
          :user='user'
          title='All Exclamations'
          :exclamations='exclamations'
          :onRemove='handleExclamationRemoval' >
        </Exclamation-List>
      </div>
      <div class="col-md-4">
        <Exclamation-List
          :user='user'
          title='Your Exclamations'
          :exclamations='userExclamations'
          :onRemove='handleExclamationRemoval' >
        </Exclamation-List>
      </div>
      <div class="col-md-4">
        <Exclamation-Search-List
          :user='user'
          title='Your Exclamations'
          :exclamations='exclamations'
          :onRemove='handleExclamationRemoval' >
        </Exclamation-List>
      </div>
    </div>
  </div>
</template>

<script>
  import axios from 'axios';
  import ExclamationList from './exclamation_list.vue';
  import ExclamationSearchList from './exclamation_search_list.vue';
  import ExclamationAddForm from './exclamation_add_form.vue';

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
      ExclamationSearchList,
      ExclamationAddForm,
    },
    methods: {
      handleExclamationRemoval(id) {
        axios.delete(`/api/exclamations/${id}`)
          .then(() => {
            this.exclamations = this.exclamations.filter(e => e.id !== id)
          });
      },
      handleExclamationAdded(text) {
        axios.post('/api/exclamations', { text }).then( ({ data }) => {
            this.exclamations = [data.exclamation].concat(this.exclamations)
        });
      },
      canAdd() {
        return this.user.scopes.includes('add');
      },
    },
    computed: {
      userExclamations () {
        return this.exclamations.filter(exc => exc.user === this.user.username);
      }
    }
  };
</script>
