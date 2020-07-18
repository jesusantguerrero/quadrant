<template>
  <div  class="container-fluid">
    <HelloWorld msg="Quadrant Technique" />
    <div class="d-flex justify-content-center">
      <ItemForm @save="addQuadrantItem" :user="user"></ItemForm>
    </div>
    <div class="btn-group">
      <button class="btn btn-primary" @click="showYesterday=!showYesterday">Show Yesterday</button>
      <button class="btn btn-primary" @click="showBacklog=!showBacklog">Show BackLog</button>
      <button class="btn btn-primary">Show Calendar</button>
      <button class="btn btn-primary">Search</button>
    </div>
    <kanbanData
      ref="Kanban"
      :data="list"
      :committed="yerterday"
      :show-yesterday="showYesterday"
      :show-backlog="showBacklog"
      @changed="updateStatus"
      @deleted="deleteItem"
      @complete-day="completeDay"
    />
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import ItemForm from "./components/ItemForm.vue"
import KanbanData from "./components/Kanban.vue"

import {reactive, ref} from "vue";

export default {
  name: 'App',
  components: {
    HelloWorld,
    ItemForm,
    KanbanData
  },
  setup(props) {
    const list = reactive([]);
    const yesterday = reactive([]);
    const showYesterday = ref(false)
    const showBacklog = ref(false)

    const updateStatus = (item) => {

    }

    const deleteItem = (item) => {

    }

    const completeDay = (item) => {

    }

    return {
      user: {
        id: 1
      },
      list,
      showYesterday,
      showBacklog,

      addQuadrantItem(item) {
        list.push(item);
      }
    }
  }
}
</script>
