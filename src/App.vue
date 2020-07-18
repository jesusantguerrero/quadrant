<template>
  <div  class="container-fluid">
    <HelloWorld msg="Quadrant Technique" />
    <div class="d-flex justify-content-center">
      <ItemForm @save="addQuadrantItem" :user="user"></ItemForm>
    </div>
    <div class="btn-group">
      <button class="btn btn-primary" @click="showYesterday=!showYesterday">Yesterday</button>
      <button class="btn btn-primary" @click="showBacklog=!showBacklog">BackLog</button>
      <button class="btn btn-primary">Show Calendar</button>
      <button class="btn btn-primary">Complete Day</button>
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
    const showYesterday = ref(true)
    const showBacklog = ref(true)

    const updateStatus = (item) => {

    }

    const deleteItem = (deletedItem) => {
      const index = list.findIndex( item => deletedItem.id == item.id);
      list.splice(index, 1);
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
      deleteItem,
      updateStatus,
      completeDay,

      addQuadrantItem(item) {
        list.push(item);
      }
    }
  }
}
</script>
