<template>
  <div id="cardTask">
    <b-card
      border-variant="secondary"
      :header="task.title"
      header-border-variant="secondary"
      align="center"
    >
      <b-card-text style="text-align: left">
        <strong>Desc:</strong>
        <br><span class="text-muted">{{ task.description ? task.description : '-' }}</span>
        <br><strong>Point:</strong>
        <br>{{ task.point }}
        <br><strong>PIC:</strong>
        <br>{{ task.pic ? task.pic : '-' }}
        <br>
        <div style="text-align: center" id="button">
          <b-button
            @click="backLog" v-show="task.status==='ToDo'"
            style="margin:5px" variant="success">&lt;&lt; Back Log</b-button>
          <b-button
            @click="toDo" v-show="task.status==='Doing'"
            style="margin:5px" variant="success">&lt;&lt; To Do</b-button>
          <b-button
            @click="doing" v-show="task.status==='Done'"
            style="margin:5px" variant="success">&lt;&lt; Doing</b-button>
          <b-button
            @click="toDo" v-show="task.status==='BackLog'"
            style="margin:5px" variant="success">To Do &gt;&gt;</b-button>
          <b-button
            @click="doing" v-show="task.status==='ToDo'"
            style="margin:5px" variant="success">Doing &gt;&gt;</b-button>
          <b-button
            @click="done" v-show="task.status==='Doing'"
            style="margin:5px" variant="success">Done &gt;&gt;</b-button>
          <br><b-button
            @click="remove"
            style="margin:5px" variant="danger">Delete</b-button>
          <!-- <b-button v-b-modal.remove variant="danger">Delete</b-button>
          <b-modal id="remove" hide-footer="true" centered title="New Task">
            asdasd
          </b-modal> -->
        </div>
      </b-card-text>
    </b-card>
  </div>
</template>

<script>
export default {
  name: 'CardTask',
  props: {
    task: Object,
    id: String,
  },
  methods: {
    remove() {
      this.$emit('remove', this.id);
    },
    backLog() {
      this.$emit('backLog', this.id);
    },
    toDo() {
      this.$emit('toDo', this.id);
    },
    doing() {
      this.$emit('doing', this.id);
    },
    done() {
      console.log('component 1');
      this.$emit('done', this.id);
      console.log('component 2');
    },
  },
};
</script>
