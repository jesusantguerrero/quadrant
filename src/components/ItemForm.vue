<template>
  <form
    action=""
    class="note-form card"
    @keydown.ctrl.s.prevent="emitSave"
    ref="QuadrantForm"
  >
    <input
      ref="Input"
      type="text"
      class="form-control"
      v-model="formData.title"
      placeholder="Keep it short"
    />
    <textarea
      v-if="focused"
      name=""
      id=""
      cols="30"
      class="form-control"
      :class="{ show: focused }"
      placeholder="Add a description"
      rows="5"
      v-model="formData.description"
    ></textarea>
    <i class="fa fa-spinner fa-spin bigger ml-2" v-if="isLoading"></i>
  </form>
</template>

<script>
import {ref, toRef, reactive, onMounted, toRefs } from "vue";

export default {
  props: {
    isLoading: {
      type: Boolean,
      default: false
    },
    user: {
      type: Object,
      default() {
        return {};
      }
    }
  },
  setup(props, { emit}) {
        // Template refs
        const Input = ref(null)
        const QuadrantForm = ref(null)

        // Data values
        const { user, isLoading } = toRefs(props);
        const focused = ref(false);
        const defaultState = ref("backlog");
        const formData = reactive({
            title: "",
            description: "",
            project: {}
        });
        const projects = reactive([
            {
                id: 1,
                name: "Oorden"
            }
        ]);

        // functions
        const prepareItem = () => {
            const id = Math.random()
                .toString(16)
                .replace(".", "");

            return {
                projectId: 1,
                commit: false,
                commitDate: "",
                userId: user.id,
                title: formData.title,
                text: formData.description || "",
                completedAt: "",
                createdAt: new Date().toISOString().slice(0, 10),
                deletedAt: null,
                updatedAt: null,
                state: defaultState.value,
                done: false,
                id: id,
                statusTimeGroup: "week"
            };
        }

        const emitSave = () => {
            focused.value = false;
            if (formData.title) {
                emit("save", prepareItem());
                formData.title = "";
                formData.description = "";
                formData.project = {};
            }
        }

        const listenClickOut = () => {
            if (!focused.value) {
                focused.value = true;
                document.addEventListener("click", e => {
                    if (!QuadrantForm.value || (e.target !== QuadrantForm.value && !QuadrantForm.value.contains(e.target) && focused.value)) {
                        focused.value = false;
                        document.removeEventListener("click", () => {});
                    }
                }, true);
            }
        }

        onMounted(() => {
            Input.value.addEventListener('focus', listenClickOut)
        })

        return {
            props,
            QuadrantForm,
            Input,
            isLoading,
            focused,
            formData,
            projects,
            defaultState,
            prepareItem,
            emitSave
        };
  }
};
</script>

<style lang="scss">
.note-form {
  width: 100%;
  max-width: 500px;
  border: 1px solid #ccc;
  overflow: hidden;
  margin: 15px 0;
  transition: all ease 0.3s;
  box-shadow: 4px 4px 4px rgba(0, 0, 0, 0.2);

  .form-control {
    border: none;
    &:focus {
      outline: none;
      box-shadow: none;
    }
  }

  .show {
    animation: ease-in appear 0.3s;
  }

  @keyframes appear {
    0% {
      height: 0;
    }
    100% {
      height: 116px;
    }
  }
}
</style>
