<template>
  <div class="backdrop">
    <form @submit="preventDefault">
      <h2>Add Task</h2>
      <label for="title">Title</label>
      <input type="text" ref="title" name="title" v-model="title">
      <label for="description">Description</label>
      <textarea ref="description" name="description" v-model="description"></textarea>
      <label for="dueDate">Due Date</label>
      <input type="date" ref="dueDate" name="dueDate" v-model="dueDate">
      <button type="button" @click="addTask">Add Task</button>
      <button type="button" @click="closeForm">Cancel</button>
    </form>

    <p>Title: {{ title }}</p>
    <p>Description: {{ description }}</p>
    <p>Due: {{ dueDate }}</p>
  </div>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
  // setup() {
    // const title = ref('')
    // const description = ref('')
    // const dueDate = ref('')

    // const preventDefault = (event: Event) => {
    //   event.preventDefault()
    // }

    // const closeModal = () => {
    //   title.value = ''
    //   description.value = ''
    //   dueDate.value = ''
    // }

    // return {
    //   title,
    //   description,
    //   dueDate,
    //   preventDefault,
    //   closeModal,
    // }
  // },
  data(){
    return{
      title:'',
      description:'',
      dueDate:''
    }
  },
  methods:{
    preventDefault(event: { preventDefault: () => void }){
      event.preventDefault()
    },
    closeForm(){
      this.$emit('close')
    },
    addTask(){
      const task = {
        id: Date.now(),
        title: this.title,
        description: this.description,
        dueDate: this.dueDate,
        status: 'pending'
      }
      this.$emit('add-task', task)
      this.closeForm()
    }
  }
})
</script>


<style scoped>
  .backdrop {
    position: fixed;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
  }
  form {
    background-color: white;
    width: 400px;
    padding: 20px;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
    display: flex;
    flex-direction: column;
    gap: 10px;
  }
  input, textarea {
    padding: 10px;
    border: 1px solid #ccc;
    border-radius: 5px;
    margin-bottom: 10px;
  }
  label{
    /* margin-bottom: 5px; */
    font-size: 14px;
  }
</style>