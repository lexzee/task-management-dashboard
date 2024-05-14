<template>
  <div class="backdrop">
    <form @submit="preventDefault">
      <h2>{{ heading }}</h2>
      <label for="title">Title</label>
      <input type="text" ref="title" name="title" v-model="title">
      <p v-if="emptyTitle" class="error">title can not be blank</p>
      <label for="description">Description</label>
      <textarea ref="description" name="description" v-model="description"></textarea>
      <p v-if="emptyDescription" class="error">description can not be blank</p>
      <label for="dueDate">Due Date</label>
      <input type="date" ref="dueDate" name="dueDate" v-model="dueDate">
      <p v-if="emptyDueDate" class="error">due date can not be blank</p>
      <button type="button" @click="addTask">{{ heading }}</button>
      <button type="button" @click="closeForm">Cancel</button>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'CustomForm',
  props: {
    heading: {
      type: String,
      required: true
    }
  },
  data(){
    return{
      title:'',
      description:'',
      dueDate:'',
      emptyTitle: false,
      emptyDescription: false,
      emptyDueDate: false
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
      if(!this.title || !this.description || !this.dueDate){
        !this.title ? this.emptyTitle = true : this.emptyTitle = false
        !this.description ? this.emptyDescription = true : this.emptyDescription = false
        !this.dueDate ? this.emptyDueDate = true : this.emptyDueDate = false
        return
      }
      const task = {
        title: this.title,
        description: this.description,
        dueDate: this.dueDate,
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
  .error {
    /* display: none; */
    margin-top: -10px;
    color: red;
    font-style: italic;
    font-size: 11px;
  }
</style>