<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input
        @keyup.enter="addTask"
        filled
        v-model="newTask"
        label="Add New Task"
        dense
        bg-color="white"
        aria-placeholder="What's your plan?"
        class="col"
        square
      >
        <template v-slot:append>
          <q-btn round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list class="bg-white" separator bordered>
      <q-item
        v-for="(task, index) in tasks"
        :key="task.item"
        @click="task.done = !task.done"
        :class="{ 'done bg-blue-1': task.done }"
        clickable
        v-ripple
      >
        <q-item-section avatar>
          <q-checkbox
            v-model="task.done"
            class="no-pointer-events"
            color="primary"
          />
        </q-item-section>
        <q-item-section>
          <q-item-label v-text="task.title"></q-item-label>
        </q-item-section>
        <q-item-section v-if="task.done" side>
          <q-btn
            @click.stop="deleteTask(index)"
            flat
            round
            dense
            color="primary"
            icon="delete"
          >
          </q-btn>
        </q-item-section>
        <q-item-section v-model="task.title" side>
          <q-btn
            @click.stop="editTask(index)"
            flat
            round
            dense
            color="primary"
            icon="edit"
          >
            <q-popup-edit v-model="task.title" :cover="false" :offset="[0, 10]">
              <q-input
                color="accent"
                v-model="task.title"
                dense
                autofocus
                counter
              >
                <template v-slot:prepend>
                  <q-icon name="record_voice_over" color="accent" />
                </template>
              </q-input>
            </q-popup-edit>
          </q-btn>
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-task absolute-center">
      <q-icon name="check" size="100px" color="primary" />
      <div class="text-h5 text-primary text-center">
        No tasks at all
      </div>
    </div>
  </q-page>
</template>

<script>
export default {
  data () {
    return {
      newTask: '',
      tasks: [
        // {
        //   title: 'Get money',
        //   done: false
        // },
        // {
        //   title: 'Eat food',
        //   done: false
        // },
        // {
        //   title: 'Poo shit',
        //   done: false
        // }
      ]
    }
  },
  methods: {
    deleteTask (index) {
      this.$q
        .dialog({
          title: 'Confirm',
          message: 'Are you sure?',
          cancel: true,
          persistent: true
        })
        .onOk(() => {
          this.tasks.splice(index, 1)
          this.$q.notify({
            message: 'Task deleted',
            color: 'purple'
          })
        })
    },
    addTask () {
      this.tasks.push({
        title: this.newTask,
        done: false
      })
      this.newTask = ''
    },
    editTask (index) {
      return {
        label: 'task.title'
      }
    }
  }
}
</script>

<style lang="scss">
.done {
  .q-item__label {
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks {
  opacity: 0.5;
}
</style>
