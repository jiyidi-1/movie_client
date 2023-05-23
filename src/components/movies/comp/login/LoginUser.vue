<template>
    <div>
        <h2>请输入您的用户密码:</h2>
        <input type="text" v-model="username" placeholder="请输入用户名" />
        <br/><br/>
        <input type="password" v-model="password" placeholder="请输入密码" />
        <br/><br/>
        <button @click="userlgoin">登录</button>
        <div v-if="status == 1">
            <h3>登陆成功,即将跳转主页</h3>
            <br><br>
            <span style="font-size: 25px;">姓名：{{user.name}}</span>
            <br><br>
            <span style="font-size: 25px;">权限：{{user.auth?"admin":"user"}}</span>
        </div>
        <div v-if="status == 2">
            <h2>请您输入正确的用户名或密码</h2>
        </div>
    </div>
</template>

<script>

export default {
    name: 'LoginUser',
    data() {
        // console.log(this)
        return {
            username : "",
            password : "",
            status : 0,
        }
    },
    props: ['user', 'login'],
    methods: {
        userlgoin() {
            if(this.username == "" || this.password == ""){
                alert("用户名或密码不能为空")
                return
            }
            var body = {
                "username": this.username,
                "password": this.password,
                "auth" : 0
            }
            this.axios.put('/user/login', body).then(res => {
                console.log(res)
                if (res.data.message == "登陆成功") {
                    var auth = -1;
                    if(res.data.data.user.userAuth == "user"){
                        auth = 0;
                    }else if(res.data.data.user.userAuth == "admin"){
                        auth = 1;
                    }
                    if(auth == -1){
                        alert("权限错误")
                        return
                    }
                    this.login(res.data.data.user.userId,res.data.data.user.userName, auth, res.data.data.user.token)

                    this.status = 1
                    setTimeout(() => {
                        this.status = 0
                    }, 2000);
                }else if(res.data.message == "401用户认证失败请查询登录"){
                    this.status = 2
                    setTimeout(() => {
                        this.status = 0
                    }, 2000);
                }
            })
        }
    }
}
</script>