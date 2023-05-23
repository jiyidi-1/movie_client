<template>
    <div id="app">
        <div class="ui">
            <MainPage v-show="page == 1" :AppPage="AppPage" :user="user" :logout="logout"></MainPage>
            <LoginPage v-show="page == 2" :AppPage="AppPage" :user="user" :login="login"></LoginPage>
            <ModifyPage v-show="page == 3" :AppPage="AppPage" :user="user"></ModifyPage>
        </div>
    </div>
</template>
  
<script>
import MainPage from './components/movies/MainPage.vue';
import LoginPage from './components/movies/LgoinPage.vue'
import ModifyPage from './components/movies/ModifyPage.vue'

export default {
    name: 'App',
    components: {
        MainPage,
        LoginPage,
        ModifyPage
    },
    data() {
        return {
            page: sessionStorage.getItem("page") || 1,
            user: JSON.parse(sessionStorage.getItem("user")) || {
                name: "请登录",
                // name: "坤先生",
                stat: 0,
                auth: 0,
                token: "",
                money: 0,
            }
        }
    },
    methods: {
        AppPage(index) {
            this.page = index;
        },
        login(id, name, auth, token) {
            this.user.id = id;
            this.user.name = name;
            this.user.stat = 1;
            this.user.auth = auth;
            this.user.token = token;

            // 查询余额
            this.axios.get('/wallet/' + this.user.id, {
                headers: {
                    token: this.user.token
                }
            }).then(res => {
                console.log("wallet@", res)
                if(res.data.message == "获取余额成功"){
                    this.user.money = res.data.data.balance;
                }
            })

            sessionStorage.setItem("user", JSON.stringify(this.user));

            // 配置统一header
            // this.axios.defaults.headers['token'] = this.user.token

            // 2s后跳转到首页
            setTimeout(() => {
                this.page = 1
            }, 2000);
        },
        logout(){
            this.user.name = "请登录";
            this.user.stat = 0;
            this.user.auth = 0;
            sessionStorage.removeItem("user");
        }
    },
    watch: {
        page: {
            handler: function (val) {
                sessionStorage.setItem("page", val);
            },
            deep: true
        },
        user: {
            handler: function (val) {
                sessionStorage.setItem("user", JSON.stringify(val));
            },
            deep: true
        }
    },
    mounted(){
        // 绑定充值后的金额更改事件
        this.$bus.$on('recharge',(balance)=>{
            // console.log('充值后金额更改',balance)
            this.user.money = balance;
            // location.reload()
        })

        // 绑定购买后的金额更改事件
        this.$bus.$on('buy_success_money',(balance)=>{
            // console.log('购买后金额更改',balance)
            this.user.money = balance;
        })
    },
    beforeDestroy() {
        this.$bus.$off('recharge')
        this.$bus.$off('buy_success_money')
    },
}
</script>
  
<style>
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
}

.ui {
    position: absolute;
    width: 100%;
    height: 100%;
    top: 0%;
    left: 0%;
    background-color: #BCC4D5;
    overflow: hidden;
}
</style>
  