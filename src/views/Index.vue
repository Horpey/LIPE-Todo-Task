<template>
  <div>
    <div class="container mt-5">
      <div class="row justify-content-center">
        <div class="col-md-5">
          <div class="appview">
              <p class="text-right small mb-2">
                  <a href="https://github.com/horpey" target="_blank">
                  &copy; Horpey 2020</a>
              </p>
            <div class="card cardstyle">
              <div class="card-body mincard">
                <p class="taskk display-inline">
                  Tasks
                  <span class="num">{{todos.length}}</span>
                </p>
                <base-button
                  @click="addTop()"
                  class="addbtn"
                  size="sm"
                  type="primary"
                  icon="fa fa-plus"
                ></base-button>
                <hr class="my-1" />
                <div class="empty" v-if="todos.length < 1">
                  <img src="/img/icons/empty.svg" />
                  <p class="small font-weight-bold text-dark">No TODO added!</p>
                </div>
                <div v-if="todos">
                  <div
                    class="card mt-3"
                    v-for="todo in todos.filter(todo => {return todo.done === false;})"
                    :key="todo.id"
                  >
                    <div class="card-body px-2 py-3">
                      <div class="row">
                        <div class="col-7 ml-2">
                          <div class="custom-control custom-checkbox">
                            <input
                              class="custom-control-input"
                              @click="oncheck(todo)"
                              :checked="todo.done"
                              :id="todo.id"
                              type="checkbox"
                            />
                            <label
                              :class="[{ 'striket': todo.done == true }, 'custom-control-label']"
                              :for="todo.id"
                            >{{todo.text}}</label>
                          </div>
                          <p class="my-0 pl-4">
                            <span class="badge badge-primary ctbadge">{{getTime(todo.id)}}</span>
                          </p>
                        </div>
                        <div class="col-4 pr-0">
                          <base-button
                            @click="removeTodo(todo.id)"
                            class="addbtn mr-0"
                            size="sm"
                            type="danger"
                            icon="fa fa-trash"
                          ></base-button>
                          <base-button
                            @click="editTodo(todo)"
                            class="addbtn mr-1"
                            size="sm"
                            type="warning"
                            icon="fa fa-pencil"
                          ></base-button>
                        </div>
                      </div>
                    </div>
                  </div>
                  <div
                    class="card mt-3"
                    v-for="todo in todos.filter(todo => {return todo.done === true;})"
                    :key="todo.id"
                  >
                    <div class="card-body px-2 py-3">
                      <div class="row">
                        <div class="col-7 ml-2">
                          <div class="custom-control custom-checkbox">
                            <input
                              class="custom-control-input"
                              @click="oncheck(todo)"
                              :checked="todo.done"
                              :id="todo.id"
                              type="checkbox"
                            />
                            <label
                              :class="[{ 'striket': todo.done == true }, 'custom-control-label']"
                              :for="todo.id"
                            >{{todo.text}}</label>
                          </div>
                          <p class="my-0 pl-4">
                            <span class="badge badge-primary ctbadge">{{getTime(todo.id)}}</span>
                          </p>
                        </div>
                        <div class="col-4 pr-0">
                          <base-button
                            @click="removeTodo(todo.id)"
                            class="addbtn mr-0"
                            size="sm"
                            type="danger"
                            icon="fa fa-trash"
                          ></base-button>
                          <base-button
                            @click="editTodo(todo)"
                            class="addbtn mr-1"
                            size="sm"
                            type="warning"
                            icon="fa fa-pencil"
                          ></base-button>
                        </div>
                      </div>
                    </div>
                  </div>
                </div>
              </div>
            </div>

            <div class="card addcard mt-3">
              <div class="card-body">
                <div v-if="(!adding && !editting)">
                  <a
                    href="javascript:void()"
                    @click="(adding = true)"
                    class="addTask font-weight-bold"
                  >
                    <span class="fa fa-plus pr-3"></span>Add Task
                  </a>
                </div>
                <div v-if="adding">
                  <form @submit.prevent="addTask">
                    <input class="form-control mb-2" required v-model="task" />
                    <div class="text-right small">
                      <a
                        href="javascript:void()"
                        @click="cancel()"
                        class="btn btn-sm btn-secondary mr-2 text-capitalize"
                      >Cancel</a>
                      <button type="submit" class="btn btn-sm btn-primary text-capitalize">Add Task</button>
                    </div>
                  </form>
                </div>

                <div v-if="editting">
                  <form @submit.prevent="editTask">
                    <input class="form-control mb-2" required v-model="editted" />
                    <div class="text-right small">
                      <a
                        href="javascript:void()"
                        @click="cancelEdit()"
                        class="btn btn-sm btn-secondary mr-2 text-capitalize"
                      >Cancel</a>
                      <button type="submit" class="btn btn-sm btn-primary text-capitalize">Edit Task</button>
                    </div>
                  </form>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
export default {
  data() {
    return {
      test: "Hello World",
      emptyState: false,
      editID: "",
      adding: false,
      editting: false,
      editted: "",
      task: "",
      todos: [
        { text: "todo 1", done: false, id: Date.now() },
        { text: "todo 2", done: true, id: Date.now() + 1 }
      ]
    };
  },
  methods: {
    addTask() {
      let text = this.task;
      this.todos.push({ text, done: false, id: Date.now() });
      this.task = "";
    },
    removeTodo(id) {
      this.todos = this.todos.filter(todo => todo.id !== id);
    },
    editTask() {
      //find the index of object from array that you want to update
      const objIndex = this.todos.findIndex(obj => obj.id === this.editID);

      // make new object of updated object.
      const updatedObj = {
        ...this.todos[objIndex],
        text: this.editted,
        id: Date.now()
      };

      // make final new array of objects by combining updated object.
      this.todos = [
        ...this.todos.slice(0, objIndex),
        updatedObj,
        ...this.todos.slice(objIndex + 1)
      ];

      this.editting = false;
    },
    editTodo(todo) {
      this.editting = true;
      this.adding = false;
      this.editID = todo.id;
      this.editted = todo.text;
    },
    oncheck(todo) {
      todo.done = !todo.done;
    },
    getTime(time) {
      var moment = require("moment");
      return moment(time).fromNow();
    },
    cancel() {
      this.adding = false;
      this.task = "";
    },
    cancelEdit() {
      this.editting = false;
      this.editted = "";
    },
    addTop() {
      this.adding = true;
      this.editting = false;
      this.task = "";
    },
    doneProject() {
      todos.filter(todo => {
        return todo.done === true;
      }).length;
    }
  }
};
</script>
<style lang="scss" scoped>
body {
  background: #f6f9fb !important;
}
.taskk {
  font-weight: bold;
}
.appview {
  min-height: 90vh;
  .cardstyle {
    box-shadow: 0 1px 2px 0 rgba(51, 132, 193, 0.302),
      0 1px 3px 1px rgba(12, 93, 154, 0.149);
    border: 0px;
    min-height: 357px;
  }
  .taskk {
    position: relative;
    display: inline-block;
    .num {
      font-weight: normal;
      font-size: 13px;
      margin-left: 4px;
      position: absolute;
    }
  }
  .addbtn {
    float: right;
  }
  .empty {
    text-align: center;
    padding: 70px 0px;
    opacity: 0.3;
    img {
      height: 80px;
    }
  }
  .f-14 {
    font-size: 14px;
    font-weight: normal;
  }
  .display-inline {
    display: inline-block;
  }
  .ctbadge {
    font-size: 9px;
    text-transform: lowercase;
    margin-left: 3px;
  }
  .addcard {
    box-shadow: 0 1px 2px 0 rgba(51, 132, 193, 0.302),
      0 1px 3px 1px rgba(12, 93, 154, 0.149);
    border: 0px;
  }
  .striket {
    text-decoration: line-through;
  }
}
.mincard {
  height: 470px;
  overflow-y: scroll;
}
@media only screen and (max-width: 768px) {
  .mincard {
    height: 590px;
    overflow-y: scroll;
  }
}
</style>
