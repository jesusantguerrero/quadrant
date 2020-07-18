<template>
  <div  class="">
    <HelloWorld msg="Quadrant Technique" />
    <div class="d-flex justify-content-center">
      <ItemForm @save="addQuadrantItem" :user="user"></ItemForm>
    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import ItemForm from "./components/ItemForm.vue"

export default {
  name: 'App',
  components: {
    HelloWorld,
    ItemForm
  },
  setup(props) {
    return {
      user: {
        id: 1
      },
      addQuadrantItem(item) {
        console.log(item);
      }
    }
  }
}
</script>
