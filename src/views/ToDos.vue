<template>
  <div>
    <h1>to do list</h1>
    <template> </template>
    <v-card class="mx-auto" max-width="600">
      <v-toolbar color="purple" dark>
        <v-app-bar-nav-icon></v-app-bar-nav-icon>

        <v-col cols="12" sm="10" class="pr-0">
          <v-text-field
            v-model.trim="newTask"
            label="What are you working on?"
            clearable
            hide-details
            @keyup.enter="addTask"
          ></v-text-field>
        </v-col>

        <v-spacer></v-spacer>

        <v-btn icon class="mr-0" @click="addTask">
          <v-icon>mdi-plus</v-icon>
        </v-btn>
      </v-toolbar>

      <v-divider></v-divider>

      <v-list flat subheader>
        <v-subheader>Tasks</v-subheader>
        <v-list-item-group v-model="settings" multiple active-class="">
          <v-list-item v-for="(task, index) in taskList" :key="index">
            <template v-slot:default="{}">
              <v-list-item-action>
                <v-checkbox :input-value="active"></v-checkbox>
              </v-list-item-action>
              <v-list-item-content
                :class="{ lineThrough: task.status === 'done' }"
              >
                <v-text-field
                  :value="task.nameTask"
                  :readonly="task.noEdit"
                  :loading="task.isLoading"
                  :ref="index"
                  dense
                ></v-text-field>
              </v-list-item-content>
              <v-btn icon class="mr-0">
                <v-icon color="purple" @click="() => editTask(index)">{{
                  task.isLoading ? "mdi-fast-forward" : "mdi-pause-circle"
                }}</v-icon>
              </v-btn>
              <v-btn icon class="mr-0" @click="() => completeTask(index)">
                <v-icon color="green">mdi-check</v-icon>
              </v-btn>
              <v-btn icon class="mr-0">
                <v-icon color="orange" @click="(e) => editTask(index)"
                  >mdi-pencil-circle-outline</v-icon
                >
              </v-btn>
              <v-btn icon class="mr-0" @click="() => deleteTask(index)">
                <v-icon color="red">mdi-close</v-icon>
              </v-btn>
            </template>
          </v-list-item>
        </v-list-item-group>
      </v-list>
    </v-card>
  </div>
</template>

<script>
export default {
  name: "to-dos",
  data() {
    return {
      taskList: [
        {
          nameTask: "Learn EnglishLearn EnglishLearn Engli",
          status: "doing",
          noEdit: true,
          isLoading: false,
        },
        {
          nameTask: "Learn Vue",
          status: "done",
          noEdit: true,
          isLoading: false,
        },
        {
          nameTask: "Learn ReactJs",
          status: "wait",
          noEdit: true,
          isLoading: false,
        },
        {
          nameTask: "Learn Nodejs",
          status: "wait",
          noEdit: true,
          isLoading: false,
        },
      ],
      newTask: "",
      noEdit: true,
      isLoading: false,
    };
  },
  methods: {
    addTask() {
      !(this.newTask.length === 0)
        ? this.taskList.push({ nameTask: this.newTask, status: "wait" })
        : null;
      this.newTask = "";
    },
    deleteTask(index) {
      this.taskList.splice(index, 1);
    },
    completeTask(index) {
      this.taskList[index].isLoading = false;
      this.taskList[index].status === "done"
        ? (this.taskList[index].status = "wait")
        : (this.taskList[index].status = "done");
    },
    editTask(index) {
      console.log(this.$refs[index][0].$el.children.chid);

      this.resetStatus(this.taskList, "isLoading");
      this.taskList[index].noEdit = false;
      this.taskList[index].isLoading = true;
      this.taskList[index].ab = true;
    },
    resetStatus(array, statusName) {
      array = array.map((val) => {
        val[statusName] = false;
      });
    },
  },
};
</script>

<style scoped>
.lineThrough {
  text-decoration: line-through;
}
</style>