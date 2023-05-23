<template>
  <div>
    <h2>点击按钮查询全体用户</h2>
    <button @click="find">查询所有用户</button>
    <div v-if="status == 1">
      <br><br>
      <pre class="information">Uid          Name          Gender          Age</pre>
      <div class="information" v-for="user in users" v-bind:key="user">
        <pre>{{user.uid}}          {{user.name}}          {{user.gender}}          {{user.age}}</pre>
      </div>
    </div>
    <div v-if="status == 2">
      <h2>没有任何用户</h2>
    </div>
  </div>
</template>

<style>
  .information{
    font-size: 24px;
    font-weight: bolder;
  }
</style>

<script>
export default {
  name: 'FindALL',
  data() {
    // console.log(this)
    return {
      status : 0,
      users:[]
    }
  },
  methods: {
    find() {
      this.axios.get('/api/v1/user/all', {
      }).then(res => {
        // console.log('post succeed')
        console.log(res)
        if (res.data.length > 0) {
          this.status = 1
          this.users = res.data
        }else{
          this.status = 2
        }
      })
    }
  }
}
</script>