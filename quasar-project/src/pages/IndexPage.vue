<template>
    

    <div class="q-pa-md q-gutter-sm">

        <q-btn color="primary" text-color="white" label="Add TODO" @click="showModelNote" />
    </div>

    <div v-if="showModel" class="overlay">
        <div class="modal">
            <!-- <p @click="showModal = false">x</p> -->
            <p class="text-todo">Title</p>
            <input type="text" class="model-input" v-model="title">

            <p class="text-todo">TODO</p>
            <textarea v-model="todo"  />
            <button @click="addTodo" class="open" >Add Note</button>
            <button @click="closeNote" class="close" >Close</button>
        </div>
    </div>

    <q-tabs v-model="selectedTab" align="justify" :full-width="true">
    <q-tab name="all">ALL</q-tab>
    <q-tab name="pending">Pending</q-tab>
    <q-tab name="complete">Complete</q-tab>
    </q-tabs>
     

  <div>
    <TodoList v-model="filteredTodoList" @update:modelValue="todos = $event"/>
  </div>

</template>

<script>
import { defineComponent, toRefs, reactive, computed } from 'vue'
import TodoList from '../components/TodoList.vue'


export default defineComponent({
  name: 'IndexPage',
  components: {
    TodoList
  },

  setup(props) {
    const data = reactive({
      selectedTab: 'all',
      title: '',
      todo: '',
      todoList: [],
      showModel: false
    })

    const addTodo = () => {
      data.todoList.push({
        title: data.title,
        text: data.todo,
        statusType: 'Pending'
      }),
      data.todo = ''
      data.title = ''
    }

    const showModelNote = () => {
      data.showModel = !data.showModel
    }

    const closeNote = () => {
      data.showModel = false
    }
    

       const filteredTodoList = computed(() => {
      switch (data.selectedTab) {
        case 'all':
          return data.todoList
        case 'pending':
          return data.todoList.filter(todo => todo.statusType === 'Pending')
        case 'complete':
          return data.todoList.filter(todo => todo.statusType === 'Complete')
        default:
          return data.todoList
      }
    })

    return {
      ...toRefs(data),
      addTodo,
      showModelNote,
      closeNote,
      filteredTodoList
    }
  }
})
</script>

<style scoped>

.overlay {
    position: absolute;
    width: 100%;
    height: 100%;
    background-color: rgba(0, 0, 0, 0.77);
    transform: translate(-50%, -50%);
    top: 50%;
    left: 50%;
    display: flex;
    align-items: center;
    justify-content: center;
    z-index: 10;
  }
  .modal {
    width: 750px;
    background-color: white;
    border-radius: 10px;
    padding: 30px;
    position: relative;
    display: flex;
    flex-direction: column;
    margin-top: 0;
  }

  .open {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: blueviolet;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 15px;
  }
  

  .close {
    padding: 10px 20px;
    font-size: 20px;
    width: 100%;
    background-color: rgb(193, 15, 15);;
    border: none;
    color: white;
    cursor: pointer;
    margin-top: 7px;
    
  }

  textarea {
    width: 100%;
    height: 200px;
    padding: 5px;
    font-size: 20px;
    margin-top: 5px;
    border: 1px solid black;
  }

  .text-todo{
    margin: 1px;
    font-weight: bold;
    font-size: 1rem;
  }

  .model-input{
    margin-bottom: 5px;
    height: 40px;
    border: 1px solid black;
  }


</style>
