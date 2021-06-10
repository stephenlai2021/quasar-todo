<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        v-model="newTask"
        @keyup.enter="addTask"
        class="col"
        filled
        bg-color="white"
        placeholder="Add task"
        dense
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" @click="addTask" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="task in tasks"
        :key="task.id"
        clickable
        @click="updateTask(task.id, task.done)"
        :class="{ 'done bg-blue-1': task.done }"
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            flat
            round
            dense
            color="primary"
            icon="delete"
            @click.stop="deleteTask(task.id)"
          />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary">
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent, ref, onMounted } from "vue";
// import getCollection from "../composables/getCollection";
import { db } from "../firebase/config";

export default defineComponent({
  // setup() {
  //   const { documents } = getCollection("todos");

  //   const deleteTask = (id) => {
  //     db.collection("todos").doc(id).delete();
  //   };

  //   const updateTask = (id, done) => {
  //     db.collection("todos").doc(id).update({ done: !done });
  //   };

  //   return {
  //     documents,
  //     deleteTask,
  //     updateTask,
  //   };
  // },
  data() {
    return {
      tasks: [],
      error: null,
      newTask: "",
    };
  },
  created() {
    db.collection("todos")
      .orderBy("createdAt")
      .onSnapshot(
        (snap) => {
          let results = [];
          snap.docs.forEach((doc) => {
            doc.data().createdAt && results.push({ ...doc.data(), id: doc.id });
          });
          this.tasks = results;
          this.error = null;
        },
        (err) => {
          this.tasks = null;
          this.error = "could not fetch data";
        }
      );
  },
  methods: {
    deleteTask(id) {
      this.$q
        .dialog({
          title: "Confirm",
          message: "Really deleted?",
          cancel: true,
          persistent: true,
        })
        .onOk(() => {
          db.collection("todos").doc(id).delete();
          this.$q.notify({ message: "Task deleted" });
        });
    },
    updateTask(id, done) {
      db.collection("todos").doc(id).update({ done: !done });
    },
    addTask() {
      db.collection('todos').add({
        title: this.newTask,
        done: false,
        createdAt: new Date().toLocaleString()
      })
      this.newTask = ''
    },
  },
});
</script>

<style lang="scss" scoped>
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks {
  opacity: 0.5;;
}
</style>
