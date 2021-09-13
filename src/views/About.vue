<template>
  <!-- <div class="about">
    <h1 style="margin-top: 30px">
      Ask a yes/no question: <input v-model="question" />
    </h1>

    <h2>{{ answer }}</h2>
    <img :src="imgUrl" alt="" />
  </div> -->

  <div id="todo-list-example">
    <p
      draggable="true"
      ondragstart="event.dataTransfer.setData('text/plain', 'This text may be dragged')"
    >
      This text <strong>may</strong> be dragged.
    </p>

    <form @submit.prevent="addNewTodo">
      <label for="new-todo">Add a todo</label>
      <br />
      <input
        v-model="newTodoText"
        id="new-todo"
        placeholder="E.g. Feed the cat"
      />
      <br />
      <!-- <button>Add</button> -->
    </form>
    <ul>
      <li
        is="todo-item"
        v-for="(todo, index) in todos"
        :key="todo.id"
        :title="todo.title"
        @remove="todos.splice(index, 1)"
      ></li>
    </ul>
  </div>
</template>

<script>
import _ from 'lodash';
import axios from 'axios';
import TodoItem from '@/components/TodoItem.vue';

export default {
  name: 'App',
  components: { TodoItem },
  data: () => ({
    question: '',
    answer: 'I cannot give you an answer until you ask a question!',
    imgUrl: '',
    newTodoText: '',
    todos: [
      {
        id: 1,
        title: 'Do the dishes',
      },
      {
        id: 2,
        title: 'Take out the trash',
      },
      {
        id: 3,
        title: 'Mow the lawn',
      },
    ],
    nextTodoId: 4,
  }),

  computed: {},
  watch: {
    // whenever question changes, this function will run
    question: function (newQuestion, oldQuestion) {
      this.answer = 'Waiting for you to stop typing...';
      this.getAnswer();
    },
  },
  created: function () {
    // _.debounce is a function provided by lodash to limit how
    // often a particularly expensive operation can be run.
    // In this case, we want to limit how often we access
    // yesno.wtf/api, waiting until the user has completely
    // finished typing before making the ajax request. To learn
    // more about the _.debounce function (and its cousin
    // _.throttle), visit: https://lodash.com/docs#debounce
    this.debouncedGetAnswer = _.debounce(this.getAnswer, 500);
  },
  methods: {
    getAnswer: _.debounce(function () {
      if (this.question.indexOf('?') === -1) {
        this.answer = 'Questions usually contain a question mark. ;-)';
        return;
      }
      this.answer = 'Thinking...';
      var vm = this;
      axios
        .get('https://yesno.wtf/api')
        .then(function (response) {
          vm.answer = _.capitalize(response.data.answer);
          vm.imgUrl = response.data.image;
        })
        .catch(function (error) {
          vm.answer = 'Error! Could not reach the API. ' + error;
        });
    }, 500),

    addNewTodo: function () {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText,
      });
      this.newTodoText = '';
    },
  },
};
</script>
