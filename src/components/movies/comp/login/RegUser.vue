<template>
    <div>
        <h2>请注册新用户:</h2>
        用户名:<input type="text" v-model="user.username" placeholder="请输入用户名" />
        <br><br>
        密码:<input type="password" v-model="user.password" placeholder="请输入密码" />
        <br><br>
        <!-- <h3>权限:</h3> -->
        用户<input type="radio" v-model="user.auth" value="user" />
        管理员<input type="radio" v-model="user.auth" value="admin" />
        <br><br>
        <button @click="userregister">添加用户</button>
    </div>
</template>

<script>
export default {
    name: 'RegUser',
    data() {
        // console.log(this)
        return {
            user: {
                username: '',
                password: '',
                auth: "user"
            }
        }
    },
    methods: {
        userregister() {
            if(this.user.username == "" || this.user.password == ""){
                alert("用户名或密码不能为空")
                return
            }
            var data = {
                "username": this.user.username,
                "password": this.user.password,
                "auth" : this.user.auth
            }
            // console.log('data:', data)
            this.axios.post('/user/register', data).then(res => {
                // console.log('post succeed')
                console.log(res)
                if (res.data.message == "创建新用户") {
                    alert("添加用户成功！")
                }else{
                    alert("用户信息违例或已存在！")
                }
            })
        }
    }
}
</script>