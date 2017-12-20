<template>
  <div class="hello">
    <h1 v-html="title"></h1>
    <p class="date">以下是{{getDate()}}要做的事情</p>
    <input v-model="newItem" @keyup.enter="addItem">
    <button @click="addItem">添加任务</button>
    <ul class="item">
      <li v-for="(item,index) in items">
        <span :class="{finished: item.isFinished,unfinished: !item.isFinished}">{{item.label}}</span>
        <button @click="toggleFinish(item)">完成</button>
        <button @click="toggleDelete(index)">删除</button>
      </li>
    </ul>
  </div>
</template>

<script>
  import Store from '../store'

export default {
  name: 'HelloWorld',
  data (){
    return {
      title: 'This is todo list',
      items: Store.fetch(),
      newItem: '',
      date: new Date()
    }
  },
  methods:{
    toggleFinish(item){
      item.isFinished = !item.isFinished
    },
    toggleDelete(index){
      this.items.splice(index, 1)
    },
    addItem(){
      this.items.push({
        label: this.newItem,
        isFinished: false
      });
      this.newItem = '';
    },
    getDate(){
      return this.date.getFullYear()+'年'
        +(this.date.getMonth()+1)+'月'
        +this.date.getDate()+'日'
    }
  },
  watch:{
      items:{
        handler(items){
          Store.save(items)
        },
        deep: true
      }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  .date{
    color: deepskyblue;
    font-size: 16px;
  }
  .item{
    font-size: 18px;
  }
  .finished{
    text-decoration: line-through;
    color: green;
  }
  .unfinished{
    color:red;
  }
</style>
