<template>
    <div>
        <h2>请输入所查询的用户ID:</h2>
        <input type="text" v-model="id" placeholder="请输入ID" />
        <button @click="find">查询用户</button>
        <div v-if="status == 1">
            <br><br>
            ID:{{user.id}}
            <br><br>
            姓名：{{user.name}}
            <br><br>
            性别：{{user.gender}}
            <br><br>
            年龄：{{user.age}}
        </div>
        <div v-if="status == 2">
            <h2>没有这个用户哦</h2>
        </div>
    </div>
</template>

<script>
export default {
    name: 'FindUser',
    data() {
        // console.log(this)
        return {
            id : 0,
            status : 0,
            user: {
                id: 0,
                name: '',
                gender: '',
                age: 0
            }
        }
    },
    methods: {
        find() {
            this.axios.get('/api/v1/user', {
                params:{
                    uid : this.id
                }
            }).then(res => {
                // console.log('post succeed')
                console.log(res)
                if (res.data) {
                    this.status = 1
                    this.user.id = res.data.uid
                    this.user.name = res.data.name
                    this.user.gender = res.data.gender
                    this.user.age = res.data.age
                }else{
                    this.status = 2
                }
            })
        }
    }
}
</script>