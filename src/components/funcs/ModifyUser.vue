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
        <button @click="modify">修改用户</button>
    </div>
</template>

<script>
export default {
    name: 'ModifyUser',
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
        modify() {
            // console.log('add run here')
            var data = {
                uid: this.user.id,
                name: this.user.name,
                gender: this.user.gender,
                age: this.user.age
            }
            this.axios.put('/api/v1/user', data).then(res => {
                // console.log('post succeed')
                console.log(res)
                if (res.data == "Modify success.") {
                    alert("修改成功！")
                }else if(res.data == "There is no such user."){
                    alert("用户不存在！")
                }
            })
        }
    }
}
</script>