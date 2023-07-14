<script>
const backEndHost = "https://opendoorapi.liuyf7.top";

export default {
//要触发响应式更新的所有变量
//禁止使用的前缀：$、_
  data() {
    return {
      isDeviceOnline: false,
      snackbar: false,
    }
  },
//包含所有方法的对象
  methods: {
    openTheDoor() {
      // this.snackbar = true;
      // return;
      fetch(backEndHost + '/api/open')
        .then(response => response.text())
        .then(data => {
          // 处理响应数据
          console.log(data);
          this.snackbar = true;
        })
        .catch(error => {
          // 处理错误
          console.error(error);
        });

    },
  },

  // 生命周期钩子
  mounted() {
    fetch(backEndHost + "/api/isalivea")
      .then(response => response.text())
      .then(data => {
        // 处理响应数据
        console.log(data);
        this.isDeviceOnline = data === "True";
      })
      .catch(error => {
        // 处理错误
        console.error(error);
      });
  },
//计算属性，可以在{{}}里面用
//当做一个变量用的时候，它是响应式的，带有缓存机制
//当做一个函数用的时候，它和普通的js函数一样
//注意：计算属性应只做计算而没有任何其他的副作用
  computed: {
    // 一个计算属性的 getter

  }
}
</script>
<template>
  <v-card class="w-50 mx-auto">
    <v-card-title>舵机运行状态</v-card-title>
    <v-card-text>
      <!--      <v-row >-->

      <v-btn color="green" v-if="isDeviceOnline" prepend-icon="mdi-access-point-check">在线</v-btn>
      <v-btn color="red" v-else prepend-icon="mdi-access-point-remove">离线</v-btn>
      <!--      </v-row>-->
    </v-card-text>
  </v-card>
  <v-card class="w-50 mx-auto">
    <v-card-text>
      <v-row justify="center">
        <v-btn large color="blue" @click="openTheDoor()" class="mx-auto">开门!</v-btn>
      </v-row>
    </v-card-text>
  </v-card>

  <v-snackbar v-model="snackbar" :timeout="1500"  color="success">
    成功发送请求
  </v-snackbar>
</template>
