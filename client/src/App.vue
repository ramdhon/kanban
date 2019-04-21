<template>
  <div id="app">
    <div id="navbar">
      <b-navbar toggleable="lg" type="dark" variant="dark">
        <router-link to="/">
          <b-navbar-brand>
            <i class="fas fa-clipboard-list"></i> KanBanDon
          </b-navbar-brand>
        </router-link>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <b-button v-b-modal.modal-1 variant="light">New Task</b-button>
          </b-navbar-nav>
        </b-collapse>
        <!-- Modal Component -->
        <dd-form-task form-id="modal-1" />
      </b-navbar>
      <br>
      <router-view/>
    </div>
  </div>
</template>

<script>
import ddFormTask from '@/components/Form.vue';

export default {
  name: 'App',
  components: {
    ddFormTask,
  },
  data() {
    return {
    };
  },
};
</script>
