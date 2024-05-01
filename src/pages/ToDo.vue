<template>
  <q-page>
    <div class="row q-pa-md">
      <q-input
        @keyup.enter="addTask"
        filled
        class="col"
        v-model="newTaskText"
        placeholder="Enter new task"
        dense
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask"></q-btn>
        </template>
      </q-input>
    </div>
    <q-list separator bordered>
      <q-item
        @click="markDone(task)"
        clickable
        :class="{ done: task.done }"
        v-for="(task, index) in tasks"
        :key="task.title"
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox v-model="task.done" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            color="primary"
            @click.stop="deleteTask(index)"
            icon="delete"
          ></q-btn>
        </q-item-section>
      </q-item>
    </q-list>
  </q-page>
</template>

<script setup>
import { ref } from "vue";
import { useQuasar } from "quasar";

//Prepare dependencies
const $q = useQuasar();

//New task text
const newTaskText = ref("");

//Tasks array
const tasks = ref([
  {
    title: "Todo 1",
    done: false,
  },
  {
    title: "Todo 2",
    done: false,
  },
]);

//Tasks CRUD handlers
const addTask = () => {
  tasks.value.unshift({
    title: newTaskText.value,
    done: false,
  });
  newTaskText.value = "";
};

const markDone = (task) => {
  task.done = !task.done;
};

const deleteTask = (index) => {
  $q.dialog({
    title: "Confirm",
    message: "Are you sure you want to delete task?",
    cancel: true,
    persistent: true,
  }).onOk(() => {
    tasks.value.splice(index, 1);
    $q.notify("Task deleted");
  });
};
</script>

<style lang="scss">
.done {
  color: primary;
  background-color: rgb(188, 251, 204);

  q-item-label {
    text-decoration: line-through;
  }
}
</style>
