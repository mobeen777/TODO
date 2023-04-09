<template>
  <div v-for="(todo, index) in modelValue" :key="index">
    <div class="q-pa-md" style="max-width: 350px" @click="showNoteDetails = true">
      <q-list bordered separator>
        <q-item clickable v-ripple>
          <q-item-section>
            <q-item-label>{{ todo.title }}</q-item-label>
            <q-item-label caption>{{ todo.statusType }}</q-item-label>
          </q-item-section>
        </q-item>
      </q-list>
    </div>
    <div class="behind" v-if="showNoteDetails">
      <div class="note-details">
        <p style="font-weight: bold; margin-bottom: 0;">Title</p>
        <p>&nbsp;&nbsp;&nbsp;&nbsp;{{ todo.title }}</p>
        <p style="font-weight: bold; margin-bottom: 0;">Details</p>
        <p>&nbsp;&nbsp;&nbsp;&nbsp;{{ todo.text }}</p>

        

        <p style="font-weight: bold; margin-bottom: 0;">Status</p>
        <p>&nbsp;&nbsp;&nbsp;&nbsp;{{ todo.statusType }}</p>
        <div class="q-pa-md q-gutter-sm">
          <q-btn color="primary" label="Change Status" @click="toggleStatus(todo)" />
          <q-btn color="primary" label="Close" @click="close"/>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { defineComponent, reactive, toRefs } from 'vue'

export default defineComponent({
  name: 'TodoList',
  props: {
    modelValue: {
      type: Array,
      default: () => []
    },
  },
  setup(props) {
    const data = reactive({
      showNoteDetails: false
    })

    const toggleStatus = (todo) => {
      todo.statusType = todo.statusType === 'Pending' ? 'Complete' : 'Pending'
      
    }

    const close = () => {
      data.showNoteDetails = false
    }
    return {
      ...toRefs(data),
      toggleStatus,
      close
    }
  }
})
</script>

<style scoped>
.behind {
  position: absolute;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.77);
  transform: translate(-50%, -50%);
  top: 30%;
  left: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}

.note-details {
  width: 750px;
  background-color: white;
  border-radius: 10px;
  padding: 30px;
  position: relative;
  display: flex;
  flex-direction: column;
  margin-top: 0;
}
</style>
