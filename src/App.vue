<template>
  <div id="app">
    <h1>{{this.title}}</h1>
    <input type="text" class="input-item" placeholder="add new item and press enter" v-model="newItem" @keyup.enter="addItem">
    <ul class="items">
      <li v-for="item in items" :class="{ item: true, finished: item.isFinished }" @click="toggleFinish(item)">
        {{item.label}}
        <button @click="deleteItem(item)" class="del-btn">del</button>
      </li>
    </ul>
  </div>
</template>

<script>
import Store from "./store";
export default {
  data() {
    return {
      title: "TODO LIST",
      items: Store.fetch(),
      newItem: ""
    };
  },
  watch: {
    items: {
      handler(items) {
        Store.save(items);
      },
      deep: true
    }
  },
  methods: {
    toggleFinish(item) {
      item.isFinished = !item.isFinished;
    },
    addItem() {
      if (this.newItem !== "") {
        this.items.push({
          label: this.newItem,
          isFinished: false
        });
        this.newItem = "";
      } else {
        alert("item can't be empty");
      }
    },
    deleteItem(item) {
      this.items = this.items.filter(it => it != item);
    }
  }
};
</script>

<style>
#app {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-150px, -350px);
  width: 300px;
  height: 400px;
  margin: 0 auto;  
  border: 1px solid #2c3e50;
  border-radius: 10px;
  text-align: center;
  color: #2c3e50;
  overflow: hidden;
}

.input-item {
  width: 80px;
  height: 20px;
  padding: 0 5px;
  border: 1px solid #2c3e50;
  border-radius: 3px;
  outline: none;
  transition: width ease 0.3s;
}
.input-item:focus {
  width: 180px;
}
.finished {
  text-decoration: line-through;
  color: #cacaca;
}
.items {
  list-style: none;
  padding: 0;
  margin: 20px 0;
}
.items .item {
  margin: 10px 3px 10px 35px;
  cursor: pointer;
}
.del-btn {
  float: right;
  cursor: pointer;
}

</style>
