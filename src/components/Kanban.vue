<template>
  <div>
    <div class="row d-flex mt-5">
      <div class="col state-card card" :class="`color-yerterday`" v-if="showYesterday">
        <h4 class="title">
          committed
        </h4>
        <div class="items-container">
          <quadrant-task
            :item="item"
            v-for="item in committed"
            :key="item.id"
            :allow-check="false"
          >
          </quadrant-task>
        </div>
      </div>

      <template
        v-for="stage in stages"
      
      >
      <div
        v-if="stage != 'backlog' || showBacklog"
        class="col state-card card"
        :class="`color-${stage}`"
        :key="stage"
      >
        <h4 class="title">
          {{ stage }} <span class="small">({{ quadrants[stage].length }})</span>
        </h4>
        <div
          :id="stage"
          v-if="quadrants[stage]"
          class="items-container"
        >
          <QuadrantTask
            v-for="item in quadrants[stage]"
            :item="item"
            :stages="stages"
            :key="item.id"
            @deleted="$emit('deleted', $event)"
            @changed="$emit('changed', $event)"
          />
        </div>
      </div>
      </template>
    </div>
  </div>
</template>

<script setup="props, { emit }">
import QuadrantTask from "./QuadrantTask.vue";
import { watch, computed, toRefs, reactive} from "vue"

export default {
  components: {
    QuadrantTask
  },
  props: {
      data: {
        type: Array,
        default() {
          return [];
        }
      },
      committed: {
        type: Array,
        default() {
          return [];
        }
      },
      showYesterday: {
        type: Boolean
      },
      showBacklog: {
        type: Boolean
      }
    }
}  
export const {data, committed, showYesterday, showBacklog} = toRefs(props);
export const quadrants =  reactive({
      backlog: [],
      do: [],
      schedule: [],
      delegate: [],
      delete: []
    });
export const stages = ["do", "schedule", "delegate", "delete", "backlog"]

watch(data, (data) => {
  stages.forEach(stage => {
    quadrants[stage] = filterByState(data, stage);
  })
}, {
  deep: true,
  immediate: true
})

const filterByState = (data, stage) => {
  return data.filter(block => block.state == stage && block.commit == false).slice();
}
</script>

<style lang="scss">
.state-card {
  min-width: 270px;
  margin: 10px 15px;
  min-height: 300px;

  &.color-delete {
    background: #f28b82;
  }

  &.color-delegate {
    background: #fff475;
  }

  &.color-schedule {
    background: #cbf0f8;
  }

  &.color-do {
    background: #ccff90;
  }

  .title {
    color: #20214e;
    text-transform: capitalize;
    text-align: center;
    padding: 10px;
  }

  .items-container {
    height: 100%;
    width: 100%;
  }
}
</style>
