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
        <ul>
          <li class="row" v-for="(todo, index) in newTodos" :key="todo.index">
            <input type="checkbox" class="cb" v-model="todo.isDone" />
            <p class="date" v-if="windowWidth > 375" >{{ todo.date }}</p>
            <p class="date" v-if="windowWidth <= 375" >{{ todo.date.substr(5) }}</p>
            <p class="memo">{{ todo.memo }}</p>
            <p>
              <button class="del_button" @click="delTodo(index)">
                <img src="~/assets/trashbox.png" />
              </button>
            </p>
          </li>
        </ul>
      </div>
    </section>
    <section class="add">
      <h2>新しい作業の追加</h2>
      <div class="add_area">
        <p>メモ</p>
        <input
          type="text"
          placeholder="内容を入力してください"
          class="text_box"
          v-model="newLine"
        />
        <button class="add_button" @click="addTodo" :disabled="textJudge()">
          追加
        </button>
      </div>
    </section>
  </div>
</template>

<script>
import "normalize.css";
export default {
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
      windowWidth: 0,
    };
  },
  mounted() {
    window.addEventListener("resize",this.getWindowSize)
  },
  methods: {
    getWindowSize() {
      this.windowWidth = window.innerWidth;
    },
    addTodo() {
      if (this.newLine == "") return;

      const getDate =()=> {
        const date = new Date();
        const Y = date.getFullYear();
        const M = ("00" + (date.getMonth() + 1)).slice(-2);
        const D = ("00" + date.getDate()).slice(-2);
        const h = ("00" + date.getHours()).slice(-2);
        const m = ("00" + date.getMinutes()).slice(-2);

        return `${Y}.${M}.${D} ${h}:${m}`
      }
      this.todos.push({ isDone: false, date: getDate(), memo: this.newLine });
      this.newLine = "";
    },
    delTodo(index) {
      this.todos.splice(index, 1);
    },
    textJudge() {
      if (this.newLine === "") return true;
      else return false;
    },
  },
  computed: {
    todoStatus() {
      let allNum = this.todos.length;
      this.newTodos = [];
      const doneTodos = this.todos.filter(todo => todo.isDone === true);
      let doneNum = doneTodos.length;
      const notDoneTodos = this.todos.filter(todo => todo.isDone === false);
      
      
      this.putNum = 0;
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
      return this.putNum;
    },
  },
};
</script>

<style scoped>
.contents {
  width: 640px;
  height: auto;
  margin: 96px auto 486px;
  font-family: "Hiragino Sans";
}
h2 {
  font-weight: 700;
  font-size: 24px;
  margin: 0;
}
h1 {
  font-weight: 700;
  font-size: 36px;
}
p {
  font-size: 14px;
}
li {
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
  font-weight: normal;
}

.todo_table ul {
  display: block;
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
  background-color: #ffffff;
}
.add_area {
  width: 456px;
  height: 32px;
  margin: 24px 0 0;
  align-items: center;
  display: flex;
  padding-left: 0;
}
.add {
  width: 456px;
  height: 92px;
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
  color: #ffffff;
  font-size: 14px;
  line-height: 100%;
  background-color: #00aaff;
  border-radius: 14px;
  border: solid;
}

@media screen and (max-width: 375px) {
  .contents {
    width: 100%;
    margin: 36px 16px 0;
  }
  h1 {
    font-size: 24px;
  }
  h2 {
    font-size: 16px;
  }
  .date {
    width: 96px;
  }
  .d_head {
    margin-left: 0px;
    width: 90px;
  }
  .memo {
    width: 133px;
    margin: 0 24px 0 0;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
  .add_area {
    width: 375px;
    flex-wrap: wrap;
  }
  .add {
    width: 456px;
    height: 92px;
  }
  .text_box {
    width: 290px;
    margin-right: 0;
  }
  .add_button {
    margin-top: 16px;
  }
}
</style>

