<template>
  <q-page>
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

const markDone = (task) => {
  task.done = !task.done;
};

const deleteTask = (index) => {
  tasks.value.splice(index, 1);
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
