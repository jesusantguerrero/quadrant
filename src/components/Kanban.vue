<template>
  <div>
    <!-- <kanban-board :stages="stages" :blocks="data"> </kanban-board> -->
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
          group="notes"
          v-bind="dragOptions"
          :id="stage"
          @start="isDragging = true"
          @end="isDragging = false"
          v-if="quadrants[stage]"
          class="items-container"
          @change="onChange($event, stage)"
        >
          <QuadrantTask
            v-for="item in quadrants[stage]"
            :item="item"
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

<script>
import draggable from "vuedraggable";
import QuadrantTask from "./QuadrantTask.vue";

export default {
  components: {
    draggable,
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
  },
  data() {
    return {
      isDragging: false,
      dialogVisible: false,
      quadrants: {
        backlog: [],
        do: [],
        schedule: [],
        delegate: [],
        delete: []
      },
      stages: ["do", "schedule", "delegate", "delete", "backlog"]
    };
  },

  watch: {
    data: {
      handler() {
        this.stages.forEach(stage => {
          this.quadrants[stage] = this.filterByState(stage);
        });
      },
      deep: true,
      immediate: true
    }
  },
  computed: {
    dragOptions() {
      return {
        animation: 0,
        group: "description",
        ghostClass: "ghost"
      };
    }
  },
  methods: {
    filterByState(state) {
      return this.data
        .filter(block => block.state == state && block.commit == false)
        .slice();
    },
    onChange({ added }, stage) {
      if (added) {
        added.element.state = stage;
        this.$emit("changed", added.element);
      }
    },
    openDialog() {
      this.dialogVisible = true;
    },
    completeDay() {
      this.dialogVisible = false;
      this.$emit("complete-day");
    }
  }
};
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
