<template>
  <div class="contents">
    <section class="title">
      <h1>チュートリアルToDoリスト</h1>
    </section>
    <section class="todo_list">
      <h2>ToDoリスト</h2>
      <ul class="status">
        <li>
          <input
            type="radio"
            id="all"
            v-model="radio"
            name="status"
            value="all"
          />
          <label for="all">すべて</label>
        </li>
        <li>
          <input
            type="radio"
            id="not_done"
            v-model="radio"
            name="status"
            value="not_done"
          />
          <label for="not_done">未完了</label>
        </li>
        <li>
          <input
            type="radio"
            id="done"
            v-model="radio"
            name="status"
            value="done"
          />
          <label for="done">完了</label>
        </li>
        <li>（{{ todoStatus }}件）</li>
      </ul>
      <div class="todo_table">
        <div class="todo_headline">
          <p class="date d_head">作成日時</p>
          <p class="memo m_head">メモ</p>
        </div>
        <ul class="row" v-for="(todo, index) in newTodos" :key="todo.date">
          <input type="checkbox" class="cb" v-model="todo.isDone" />
          <li class="date">{{ todo.date }}</li>
          <li class="memo">{{ todo.memo }}</li>
          <li>
            <button class="del_button" @click="delTodo(index)">
              <img src="../static/trashbox.png" />
            </button>
          </li>
        </ul>
      </div>
    </section>
    <section class="add">
      <h2>新しい作業の追加</h2>
      <ul class="add_area" submit.prevent>
        <p>メモ</p>
        <input
          type="text"
          value=""
          placeholder="内容を入力してください"
          class="text_box"
          v-model="newLine"
          required
        />
        <button class="add_button" @click="addTodo">追加</button>
      </ul>
    </section>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      radio: "all",
      todos: [
        {
          isDone: true,
          date: "2022.09.22 18:23",
          memo: "買い物：鶏むね肉・人参・玉ねぎ・じゃがいも・ルー・福...",
        },
        { isDone: false, date: "2022.09.23 01:23", memo: "ゴミ捨て" },
      ],
      newTodos: [],
      newLine: "",
      putNum: 0,
    };
  },
  methods: {
    addTodo: function () {
      if (this.newLine == "") return;
      const dateStr = getDate();
      function getDate() {
        const date = new Date();
        const Y = date.getFullYear();
        const M = ("00" + (date.getMonth() + 1)).slice(-2);
        const D = ("00" + date.getDate()).slice(-2);
        const h = ("00" + date.getHours()).slice(-2);
        const m = ("00" + date.getMinutes()).slice(-2);
        const s = ("00" + date.getSeconds()).slice(-2);

        return Y + "." + M + "." + D + " " + h + ":" + m;
      }
      this.todos.push({ isDone: false, date: dateStr, memo: this.newLine });
      this.newLine = "";
    },
    delTodo: function (index) {
      this.todos.splice(index, 1);
    },
  },
  computed: {
    todoStatus: function () {
      var doneNum = 0;
      var allNum = 0;
      this.putNum = 0;
      this.newTodos = [];
      var doneTodos = [];
      var notDoneTodos = [];

      for (const todo of this.todos) {
        if (todo.isDone === true) {
          doneNum += 1;
          doneTodos.push(todo);
        } else {
          notDoneTodos.push(todo);
        }
      }
      allNum = this.todos.length;
      if (this.radio === "all") {
        this.putNum = allNum;
        this.newTodos = this.todos;
      } else if (this.radio === "not_done") {
        this.putNum = allNum - doneNum;
        this.newTodos = notDoneTodos;
      } else {
        this.putNum = doneNum;
        this.newTodos = doneTodos;
      }
      return this.newTodos, this.putNum;
    },
  },
};
</script>

<style scoped>
.contents {
  width: 640px;
  height: auto;
  margin: 96px auto 486px auto;
  font-family: "Hiragino Sans";
}
.contents h2 {
  font-weight: 700;
  font-size: 24px;
  margin: 0;
}
.contents h1 {
  font-weight: 700;
  font-size: 36px;
}
.contents p {
  font-size: 14px;
}
.contents li {
  list-style: none;
}
.todo_list {
  margin: 48px 0;
}
.todo_list ul {
  display: flex;
  margin: 24px 0;
  padding-left: 0px;
}
.status li {
  margin-right: 24px;
}
.todo_table {
  font-size: 14px;
  font-weight: 300;
  font-weight: normal;
}
.todo_headline {
  display: flex;
  margin: 0;
  padding: 0px 45px;
}
.date {
  width: 144px;
  margin: 0 24px;
  text-align: left;
}
.d_head {
  margin-left: 0px;
}
.memo {
  width: 382px;
  margin: 0 25.75px 0 0;
}
.row {
  display: flex;
  margin-top: 16px;
  height: 21px;
  align-items: center;
}
.del_button {
  width: 17.5px;
  height: 20px;
  border: none;
  background-color: white;
}
.add_area {
  width: 456px;
  height: 32px;
  margin: 0;
  align-items: center;
}
.add {
  width: 456px;
  height: 92px;
}
.add ul {
  display: flex;
  padding-left: 0;
  margin-top: 24px;
}
.add p {
  width: 30px;
  margin: 0;
}
.text_box {
  margin: 0 24px;
  width: 320px;
}
.add_button {
  width: 64px;
  height: 27px;
  color: white;
  font-size: 14px;
  line-height: 100%;
  background-color: #00aaff;
  border-radius: 14px;
  border: solid;
}
</style>

