<template>
  <div class="backdrop">
    <form @submit="preventDefault">
      <h2>{{ heading }}</h2>

      <!-- Add the v-model directives to the input fields -->
      <!-- Enter Title -->
      <label for="title">Title</label>
      <input type="text" ref="title" name="title" v-model="title">
      <p v-if="emptyTitle" class="error">title can not be blank</p>

      <!-- Enter Description -->
      <label for="description">Description</label>
      <textarea ref="description" name="description" v-model="description"></textarea>
      <p v-if="emptyDescription" class="error">description can not be blank</p>

      <!-- Enter Due Date -->
      <label for="dueDate">Due Date</label>
      <input type="date" ref="dueDate" name="dueDate" v-model="dueDate">
      <p v-if="emptyDueDate" class="error">due date can not be blank</p>

      <!-- Add the submitForm method to the form's submit event -->
      <!-- <button type="button" @click="addTask">{{ heading }}</button> -->
      <button type="button" @click="submitForm">{{ heading }}</button>
      <button type="button" @click="closeForm">Cancel</button>
    </form>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue'

// Add the Task interface
interface Task {
  title: string
  description: string
  dueDate: string
}

export default defineComponent({
  name: 'CustomForm',
  props: {
    heading: {
      type: String,
      required: true
    },

    // Add the prop for the task
    mode: {
      type: String as PropType<'add' | 'edit'>,
      required: true
    },
    task: {
      type: Object as PropType<Task>,
      default: () => {
        return {
          title: '',
          description: '',
          dueDate: ''
        }
      }
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

    // Add the submitForm method
    submitForm(){
      if(!this.title || !this.description || !this.dueDate){
        !this.title ? this.emptyTitle = true : this.emptyTitle = false
        !this.description ? this.emptyDescription = true : this.emptyDescription = false
        !this.dueDate ? this.emptyDueDate = true : this.emptyDueDate = false
        return
      }
      if (this.mode === 'add') {
        const task = {
          title: this.title,
          description: this.description,
          dueDate: this.dueDate,
        }
        this.$emit('add-task', task)
      } else if (this.mode === 'edit') {
        this.$emit('edit-task',this.task,  {
          title: this.title,
          description: this.description,
          dueDate: this.dueDate
        })
      }
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