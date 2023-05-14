<template>
  <div id="app">
    <div class="">
      {{ ero }}
    </div>
    <img src="./assets/logo.png" @click="open" alt="">

    <input type="text" name="" id="" v-model="title" class="inp" placeholder="请输入查询语句">

    <div class="" v-for="(item, index) in data" :key='index'>
      <div class="aa" v-show="item.role == 'user'">
        {{ item.content }}
      </div>
      <div class="" v-show="item.role == 'assistant'">
        <div class="" v-for="(res, index) in item.content" :key='index'>
          <p> {{ res }}</p>

        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'App',
  data() {
    return {
      ero: '1',
      title: '',
      datas: [
        // {
        // role: 'user',
        // content: '网站编辑'
        // 	},
        // {
        // 	role: 'assistant',
        // 	content: '您好！请问您有什么问题需要我回答吗？'
        // },
      ],
      nav: '',
      data: [],
    }
  },
  methods: {
    open() {
      this.datas.push({
        role: 'user',
        content: this.title
      })
      this.data.push({
        role: 'user',
        content: this.title
      })
      console.log(`output->data`, data)
      this.title = ''

      console.log(`output->data`, data)
      let that = this
      console.log(this.title);
      var data = JSON.stringify({
        "model": "gpt-3.5-turbo",
        "messages": this.datas //messages就是你发的消息是数组形式
      });
      var config = {
        method: 'post',
        url: 'https://api.openai-proxy.com/v1/chat/completions',
        headers: {
          'Authorization': 'Bearer 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',
          'Content-Type': 'application/json',
        },
        data: data
      };
      axios(config)
        .then(function (response) {
          console.log(response.data.choices);
          // console.log(response.data.choices[0].message.content.split('\n'));
          that.nav = response.data.choices[0].message.content.split('\n');

          that.datas.push({
            role: 'assistant',
            content: response.data.choices[0].message.content
          })
          that.data.push({
            role: 'assistant',
            content: response.data.choices[0].message.content.split('\n')
          })

        })
        .catch(function (error) {
          console.log(error);
          that.ero = error
        });
    }
  },
  components: {
  }
}

</script>

<style >
.inp {
  background-color: skyblue;
}

.aa {
  color: red;
}
</style>
