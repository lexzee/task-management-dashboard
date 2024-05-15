<template>
  <div class="header">
    <div class="hNav">
      <h1>{{ title}}</h1>
      <div class="nav">
        <ul>
          <li @click="showAll" :class="{'active' : showAll === true}">All</li>
          <li @click="showCompleted" :class="{'active' : showCompleted === true}">Completed</li>
          <li @click="showPending" :class="{'active' : showPending === true}">Pending</li>
        </ul>
      </div>
    </div>
    <button @click="toggleShowAddTask">Add Task +</button>
  </div>
  <div v-if="showAddTask">
    <Form
      heading='Add Task'
      @close="toggleShowAddTask"
      @add-task="addNewTask"
      @edit-task="editTask"
      mode="add"
    />
  </div>
  <div class="tasks">
    <h2>
      <span v-if="showAllTasks">All Tasks</span>
      <span v-if="showCompletedTasks">Completed Tasks</span>
      <span v-if="showPendingTasks">Pending Tasks</span>
    </h2>
    <ul v-if="showAllTasks">
      <li v-for="task in tasks" :key="task.id" class="task" :class="{ 'completed' : task.status === 'completed'}">
        <div v-if="showEditTask">
          <Form
            heading='Edit Task'
            @close="toggleShowEditTask"
            @edit-task="editTask"
            mode="edit"
            :task="task"
          />
        </div>
        <div class="head">
          <h3>{{ task.title }}</h3>
          <div class="actions">
            <button @click="toggleShowEditTask" v-if="task.status === 'pending'">Edit</button>
            <button @click="deleteTask(task)">Delete</button>
            <input
              type="checkbox"
              name="done"
              @change="toggleStatus(task)"
              :checked="task.status === 'completed' ? true : false"
            >
          </div>
        </div>
        <div class="body">
          <p>{{ task.description }}</p>

          <span class="details">
            <p>Due Date: {{ task.dueDate }}</p>
            <p>Status: {{ task.status }}</p>
          </span>
        </div>
      </li>
    </ul>

    <!-- Completed Tasks -->
    <ul v-if="showCompletedTasks">
      <li v-for="task in completedTasks" :key="task.id" class="task" :class="{ 'completed' : task.status === 'completed'}">
        <div class="head">
          <h3>{{ task.title }}</h3>
          <div class="actions">
            <button @click="deleteTask(task)">Delete</button>
            <input
              type="checkbox"
              name="done"
              @change="toggleStatus(task)"
              :checked="task.status === 'completed' ? true : false"
            >
          </div>
        </div>
        <div class="body">
          <p>{{ task.description }}</p>

          <span class="details">
            <p>Due Date: {{ task.dueDate }}</p>
            <p>Status: {{ task.status }}</p>
          </span>
        </div>
      </li>
    </ul>

    <!-- Pending Tasks -->
    <ul v-if="showPendingTasks">
      <li v-for="task in pendingTasks" :key="task.id" class="task" :class="{ 'completed' : task.status === 'completed'}">
        <div v-if="showEditTask">
          <Form
            heading='Edit Task'
            @close="toggleShowEditTask"
            @edit-task="editTask"
            mode="edit"
            :task="task"
          />
        </div>
        <div class="head">
          <h3>{{ task.title }}</h3>
          <div class="actions">
            <button @click="toggleShowEditTask">Edit</button>
            <button @click="deleteTask(task)">Delete</button>
            <input
              type="checkbox"
              name="done"
              @change="toggleStatus(task)"
              :checked="task.status === 'completed' ? true : false"
            >
          </div>
        </div>
        <div class="body">
          <p>{{ task.description }}</p>

          <span class="details">
            <p>Due Date: {{ task.dueDate }}</p>
            <p>Status: {{ task.status }}</p>
          </span>
        </div>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Task from './types/Task'
import Form from './components/Form.vue'

export default defineComponent({
  name: 'App',
  components: {
    Form
  },
  data() {
    return {
      title: 'Teachmate',
      showAllTasks: true as boolean,
      showCompletedTasks: false as boolean,
      showPendingTasks: false as boolean,
      showAddTask: false as boolean,
      showEditTask: false as boolean,
      taskToEdit: { id: 0, name: '' },
      tasks: [
        {
          id: 1,
          title: "Task 1",
          description: "Description for Task 1",
          dueDate: "2022-01-01",
          status: "pending"
        },
        // Add more tasks here...
      ]

    };
  },
  methods: {
    // Add methods here...
    showAll() {
      this.showAllTasks = true;
      this.showCompletedTasks = false;
      this.showPendingTasks = false;
    },
    showCompleted() {
      this.showAllTasks = false;
      this.showCompletedTasks = true;
      this.showPendingTasks = false;
    },
    showPending() {
      this.showAllTasks = false;
      this.showCompletedTasks = false;
      this.showPendingTasks = true;
    },
    deleteTask(task: Task) {
      this.tasks = this.tasks.filter((t) => t.id !== task.id);
    },

    editTask(t: Task, data?: Task) {
      this.tasks = this.tasks.map((task) => {
        if (task.id === t.id) {
          return {
            ...task,
            ...data
          };
        }
        return task;
      });
    },

    addNewTask(task: Task) {
      this.tasks.push({
        ...task,
        id: Date.now(),
        status: 'pending'
      });
    },
    toggleShowAddTask() {
      this.showAddTask = !this.showAddTask;
    },
    toggleShowEditTask() {
      this.showEditTask = !this.showEditTask;
    },
    toggleStatus(task : Task) {
      if (task.status === 'completed') {
        task.status = 'pending';
      } else {
        task.status = 'completed';
      }
    },
  },
  computed: {
    completedTasks(): Task[] {
      const tasks = this.tasks.filter((task) => task.status === 'completed');
      return tasks;
    },
    pendingTasks(): Task[] {
      const tasks = this.tasks.filter((task) => task.status === 'pending');
      return tasks;
    }
  }
});
</script>

<style scoped>
  .header {
    display: flex;
    align-items: flex-start;
    justify-content: space-between;
    width: 93%;
    color: #333;
    margin: 20px auto;
    font-weight: 600;
    padding: 0 10px;
  }

  .nav ul{
    display: flex;
    gap: 20px;
    align-items: center;
  }

  .hNav{
    display: flex;
    flex-direction: column;
    justify-content: flex-start;
    gap: 20px;
  }

  .nav{
    font-size: .8rem;
  }

  .tasks{
    padding: 10px;
    width: 95%;
    margin: auto;
  }

  .task{
    display: flex;
    flex-direction: column;
    padding: 10px;
    border-radius: 10px;
    box-shadow: 1px 0 10px rgba(0, 0, 0, 0.1);
    margin: 20px auto;
  }

  .head{
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .actions{
    display: flex;
    gap: 10px;
  }

  .head .actions button{
    padding: 2.5px 5px;
    background-color: #333;
    color: #fff;
    border: none;
    border-radius: 5px;
    cursor: pointer;
  }

  .completed{
    background: #85b8ff;
  }

  .details{
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 12px;
    font-style: italic;
  }

  .active{
    color: red;
  }

  @media (width > 768px) {
    .header {
      align-items: center;
      width: 760px;
    }

    .hNav {
      flex-direction: row;
      align-items: center;
      justify-content: space-between;
      width: 84%;
    }

    .nav{
      font-size: 1rem;
    }
    /* .nav ul {
      display: flex;
      list-style: none;
      gap: 20px;
      align-items: center;
    } */

    .tasks {
      padding: 20px;
    }

    .task {
      padding: 20px;
    }

    /* .head{
      display: flex;
      justify-content: space-between;
      align-items: center;
    } */

    .head .actions button {
      padding: 5px 10px;
    }

    /* .actions{
      display: flex;
      gap: 10px;
    } */
  }
</style>
