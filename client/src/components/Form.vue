<template>
  <div id="form">
    <b-form @submit.prevent="onSubmit" @reset.prevent="onReset" v-if="show">
      <b-form-group id="input-group-1" label="Title:" label-for="input-1">
        <b-form-input
          id="input-1"
          v-model="form.title"
          required
          placeholder="Enter title"
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-2" label="Description:" label-for="textarea ">
        <b-form-textarea
          id="textarea"
          v-model="form.description"
          placeholder="Enter description"
          rows="3"
          max-rows="6"
        ></b-form-textarea>
      </b-form-group>
      <b-form-group id="input-group-3" label="Point:" label-for="input-3">
        <b-form-input
          id="input-3"
          v-model="form.point"
          placeholder="Enter point"
        ></b-form-input>
      </b-form-group>
      <b-form-group id="input-group-4" label="PIC:" label-for="input-4">
        <b-form-input
          id="input-4"
          v-model="form.pic"
          placeholder="Enter PIC"
        ></b-form-input>
      </b-form-group>

      <b-button style="margin:0 5px" type="submit" variant="success">Submit</b-button>
      <b-button style="margin:0 5px" type="reset" variant="danger">Reset</b-button>
    </b-form>
  </div>
</template>

<script>
import db from '@/api/firebase';

export default {
  name: 'Form',
  data() {
    return {
      form: {
        title: '',
        description: '',
        point: 0,
        pic: '',
      },
      show: true,
    };
  },
  methods: {
    onSubmit() {
      // this.$emit('onSubmit', {
      //   title: this.form.title,
      //   description: this.form.description,
      //   point: this.form.point,
      //   pic: this.form.pic,
      // });
      db.collection('tasks').add({
        title: this.form.title,
        description: this.form.description,
        point: Number(this.form.point),
        pic: this.form.pic,
        createdAt: new Date(),
        status: 'BackLog',
      })
        .then((docRef) => {
          console.log('Document written with ID: ', docRef.id);
        })
        .catch((error) => {
          console.error('Error adding document: ', error);
        });
      this.onReset();
    },
    onReset() {
      // Reset our form values
      this.form.title = '';
      this.form.description = '';
      this.form.point = 0;
      this.form.pic = '';
      // Trick to reset/clear native browser form validation state
      this.show = false;
      this.$nextTick(() => {
        this.show = true;
      });
    },
  },
};
</script>
