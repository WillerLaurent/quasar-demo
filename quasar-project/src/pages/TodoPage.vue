<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        class="col"
        square
        filled
        bg-color="white"
        v-model="newTask"
        @keyup.enter="addTask"
        placeholder="Add task"
        dense
      >
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.title"
        @click="task.done = task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        tag="label"
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          />
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script setup>
import { ref, reactive, onMounted } from "vue";
import { useQuasar } from "quasar";

const $q = useQuasar();

const task = ref("toto");
const newTask = ref("");
const tasks = reactive([
  {
    title: "Get bananas",
    done: false,
  },
  {
    title: "Eat Bananas",
    done: true,
  },
  {
    title: "Poo Bananas",
    done: false,
  },
]);

//#region methods ----------------------
function deleteTask(index) {
  $q.dialog({
    title: "Confirm",
    message: "Really delete ?",
    cancel: true,
    persistent: true,
  }).onOk(() => {
    tasks.splice(index, 1);
    $q.notify("Task deleted");
  });
}

function addTask() {
  //this.tasks.push
}
//#endregion methods ----------------------

onMounted(() => {
  $q.notify("mounted");
});

/* export default defineComponent({
  name: "TodoPage",
  data() {
    return {
      newTask: "",
      tasks: [
        {
          title: "Get bananas",
          done: false,
        },
        {
          title: "Eat Bananas",
          done: true,
        },
        {
          title: "Poo Bananas",
          done: false,
        },
      ],
    };
  },
  methods: {
    deleteTask(index) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Really delete ?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          this.tasks.splice(index, 1);
          this.$q.notify("Task deleted");
        });
    },
    addTask() {
      this.tasks.push    },
        },
}); */
</script>



<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
</style>