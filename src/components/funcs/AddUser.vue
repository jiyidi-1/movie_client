<template>
    <div>
        ID:<input type="text" v-model="user.id" placeholder="请输入ID" />
        <br><br>
        姓名：<input type="text" v-model="user.name" placeholder="请输入姓名" />
        <br><br>
        性别：<input type="text" v-model="user.gender" placeholder="请输入性别" />
        <br><br>
        年龄：<input type="text" v-model="user.age" placeholder="请输入年龄" />
        <br><br>
        <button @click="add">添加用户</button>
    </div>
</template>

<script>
export default {
    name: 'AddUser',
    data() {
        // console.log(this)
        return {
            user: {
                id: 0,
                name: '',
                gender: '',
                age: 0
            }
        }
    },
    methods: {
        add() {
            // console.log('add run here')
            var data = {
                uid: this.user.id,
                name: this.user.name,
                gender: this.user.gender,
                age: this.user.age
            }
            console.log('data:', data)
            this.axios.post('/api/v1/user', data).then(res => {
                // console.log('post succeed')
                console.log(res)
                if (res.data) {
                    alert("添加用户成功！")
                }else{
                    alert("用户信息违例或已存在！")
                }
            })
        }
    }
}
</script>