<template>
  <h1>{{ msg }}</h1>
</template>

<script>
import { ref, toRefs } from "vue"

export default {
  props: {
    msg: {
      type: String
    }
  },
  setup(props) {
    const count = ref(0)
    const { msg } = toRefs(props)
    return {
      count,
      msg
    }
  }
}
</script>
