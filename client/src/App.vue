<template>
  <div id="app">
    <div id="navbar">
      <b-navbar toggleable="lg" type="dark" variant="dark">
        <b-navbar-brand href="#">KanBanDon</b-navbar-brand>

        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>

        <b-collapse id="nav-collapse" is-nav>
          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <b-button v-b-modal.modal-1 variant="light">New Task</b-button>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>

      <!-- Modal Component -->
      <b-modal hide-footer="true" id="modal-1" centered title="New Task">
        <dd-form-task />
      </b-modal>
      <br>
      <b-container-fluid class="bv-example-row">
          <div id="cardsCategory">
            <b-card-group deck>
              <b-card
                v-for="(card, index) in cardsCategory"
                :key="index"
                :border-variant="card.variant"
                :header="card.header"
                :header-bg-variant="card.variant"
                header-text-variant="white"
                align="center"
              >
                <dd-card-task
                  v-for="(task, index) in card.array"
                  @backLog="backLog"
                  @toDo="toDo"
                  @doing="doing"
                  @done="done"
                  @remove="remove"
                  :key="index"
                  :task="task.data()"
                  :id="task.id"
                />
              </b-card>
            </b-card-group>
          </div>
      </b-container-fluid>
    </div>
  </div>
</template>

<script>
import ddFormTask from '@/components/Form.vue';
import ddCardTask from '@/components/CardTask.vue';

import db from '@/api/firebase';

export default {
  components: {
    ddFormTask,
    ddCardTask,
  },
  data() {
    return {
      cardsCategory: [
        {
          header: 'Back Log',
          variant: 'danger',
          array: [],
        },
        {
          header: 'To Do',
          variant: 'warning',
          array: [],
        },
        {
          header: 'Doing',
          variant: 'primary',
          array: [],
        },
        {
          header: 'Done',
          variant: 'success',
          array: [],
        },
      ],
    };
  },
  created() {
    this.initiate();
  },
  methods: {
    // onSubmit(form) {
    //   this.cardsCategory[0].array.push(form);
    // },
    initiate() {
      db.collection('tasks')
        .orderBy('createdAt', 'desc')
        .onSnapshot((querySnapshot) => {
          console.log('Data changed');
          /* eslint no-param-reassign: ["error", { "props": false }] */
          this.cardsCategory.map((card) => {
            card.array = [];
            return card;
          });
          querySnapshot.forEach((doc) => {
            // console.log(doc.id, doc.data());
            switch (doc.data().status) {
              case 'BackLog':
                this.cardsCategory[0].array.push(doc);
                break;
              case 'ToDo':
                this.cardsCategory[1].array.push(doc);
                break;
              case 'Doing':
                this.cardsCategory[2].array.push(doc);
                break;
              case 'Done':
                this.cardsCategory[3].array.push(doc);
                break;
              default:
                break;
            }
          });
        });
    },
    remove(id) {
      db.collection('tasks').doc(id).delete()
        .then(() => {
          console.log('Document removed');
          // this.initiate();
        })
        .catch((error) => {
          console.error('Error removing document: ', error);
        });
    },
    backLog(id) {
      db.collection('tasks').doc(id).set({
        status: 'BackLog',
      }, { merge: true })
        .then(() => {
          console.log('Document updated');
          // this.initiate();
        })
        .catch((error) => {
          console.error('Error updating document: ', error);
        });
    },
    toDo(id) {
      db.collection('tasks').doc(id).set({
        status: 'ToDo',
      }, { merge: true })
        .then(() => {
          console.log('Document updated');
          // this.initiate();
        })
        .catch((error) => {
          console.error('Error updating document: ', error);
        });
    },
    doing(id) {
      db.collection('tasks').doc(id).set({
        status: 'Doing',
      }, { merge: true })
        .then(() => {
          console.log('Document updated');
          // this.initiate();
        })
        .catch((error) => {
          console.error('Error updating document: ', error);
        });
    },
    done(id) {
      console.log('induk done');
      db.collection('tasks').doc(id).set({
        status: 'Done',
      }, { merge: true })
        .then(() => {
          console.log('Document updated');
          // this.initiate();
        })
        .catch((error) => {
          console.error('Error updating document: ', error);
        });
    },
  },
};
</script>
