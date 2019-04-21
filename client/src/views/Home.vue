<template>
  <div class="home">
    <b-container fluid class="bv-example-row">
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
    </b-container>
  </div>
</template>

<script>
// @ is an alias to /src
import ddCardTask from '@/components/CardTask.vue';

import db from '@/api/firebase';

import Swal from 'sweetalert2';

const swalWithBootstrapButtons = Swal.mixin({
  customClass: {
    confirmButton: 'btn btn-success',
    cancelButton: 'btn btn-danger',
  },
  buttonsStyling: false,
});

export default {
  name: 'Home',
  components: {
    ddCardTask,
  },
  created() {
    this.initiate();
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
      swalWithBootstrapButtons.fire({
        title: 'Are you sure?',
        text: "You won't be able to revert this!",
        type: 'warning',
        showCancelButton: true,
        confirmButtonText: 'Yes, delete it!',
        cancelButtonText: 'No, cancel!',
        reverseButtons: true,
      }).then((result) => {
        if (result.value) {
          db.collection('tasks').doc(id).delete()
            .then(() => {
              // console.log('Document removed');
              // this.initiate();
              swalWithBootstrapButtons.fire(
                'Deleted!',
                'Your task has been deleted.',
                'success',
              );
            })
            .catch((error) => {
              console.error('Error removing document: ', error);
              Swal.fire({
                position: 'center',
                type: 'error',
                title: '500 - Internal Server Error',
                showConfirmButton: false,
                timer: 1500,
              });
            });
        } else if (
          // Read more about handling dismissals
          result.dismiss === Swal.DismissReason.cancel
        ) {
          swalWithBootstrapButtons.fire(
            'Cancelled',
            'Your task is safe :)',
            'error',
          );
        }
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
