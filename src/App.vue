<template>
  <div class="todoapp">
    <TodoHeader @add="addFn"></TodoHeader>
    <TodoMain :list="showList" @del="delFn"></TodoMain>
    <TodoFooter :count="count" @filterdata="filterdataFn" @clear="clearFn"></TodoFooter>
  </div>
</template>

<script>
import TodoHeader from "./components/TodoHeader";
import TodoMain from "./components/TodoMain";
import TodoFooter from "./components/TodoFooter";

export default {
  data() {
    return {
      // list: [
      //   { id: 100, name: "吃饭", isDone: true },
      //   { id: 101, name: "睡觉", isDone: true },
      //   { id: 102, name: "打豆豆", isDone: false },
      //   { id: 103, name: "小樊按摩", isDone: true },
      // ],
      list: JSON.parse(localStorage.getItem("list")) || [],
      getSel: 'all'
    }
  },
  components: {
    TodoHeader,
    TodoFooter,
    TodoMain
  },
  computed: {
    showList() {
      if (this.getSel == 'no') {
        return this.list.filter((ele) => !ele.isDone)
      }
      else if (this.getSel == 'yes') {
        return this.list.filter((ele) => ele.isDone)
      }
      else {
        return this.list
      }
    },
    count() {
      return this.list.filter((ele) => !ele.isDone).length
    }
  },
  methods: {
    addFn(val) {
      const id = this.list[this.list.length - 1] ? this.list[this.list.length - 1].id + 1 : 100
      this.list.push({
        id,
        name: val,
        isDone: false,
      })
    },
    delFn(id) {
      const index = this.list.findIndex((ele) => ele.id == id)
      this.list.splice(index, 1)
    },
    filterdataFn(val) {
      this.getSel = val
    },
    clearFn() {
      this.list.forEach((ele) => (ele.isDone = false))
    }
  },
  watch: {
    list: {
      deep: true,
      handler(val) {
        localStorage.setItem('list', JSON.stringify(val || []))
      }
    }
  }
}
</script>

<style>
</style>