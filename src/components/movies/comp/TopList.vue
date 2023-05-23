<template>
    <div class="header">
        <div class="logo">
            <img src="@/assets/play.jpg" alt="logo" style="">
        </div>

        <div class="choose">
            <div><button @click="changecinema">主页</button></div>
            <div><button @click="changemovie">电影</button></div>
            <!-- <div><button @click="modify">管理员</button></div> -->
            <div><button v-if="user.auth == 1" @click="modify">管理员</button></div>
        </div>

        <div class="user">
            <div class="d1">
                <span @click="show_ticket" style="cursor:pointer">您好，{{ user.name }}</span>
            </div>
            <div class="d1" v-if="user.stat && user.auth == 0">
                <span @click="show_wallet" style="cursor:pointer">余额：{{ user.money }}</span>
            </div>
            <div class="d2" v-if="user.stat == 0"><button @click="login">登录/注册</button></div>
            <div class="d2" v-if="user.stat == 1"><button @click="logout">登出</button></div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TopList',
    data() {
        return {
            
        }
    },
    props: ['MainList', 'AppPage', "user", "logout"],
    methods: {
        changecinema() {
            this.MainList(1);
        },
        changemovie() {
            this.MainList(2);
        },
        modify(){
            this.AppPage(3);
        },
        login(){
            this.AppPage(2);
        },
        show_ticket(){
            if(this.user.stat == 0){
                alert("请先登录\n再查看订单信息");
                return;
            }
            this.$bus.$emit("show_main_page", 5)
        },
        show_wallet(){
            this.$bus.$emit("show_main_page", 4)
        }
    }
}
</script>

<style scoped lang="less">
.header {
    display: inline-block;
    position: fixed;
    width: 98%;
    height: 10%;
    top: 0%;
    left: 1%;
    background-color: bcc4d5;
    overflow: hidden;
    display: flex;
}

.logo {
    position: relative;
    height: 100%;
    width: 20%;
    left: 0%;

    img {
        height: 100%;
        float: left;
        border-radius: 20px;
    }
}

.choose {
    position: relative;
    height: 100%;
    width: 40%;
    left: 0%;
    display: flex;
    justify-content: center;
    align-items: center;
    // border: red solid 1px;

    div{
        width: 30%;
    }

    button {
        position: relative;
        background-color: #96B97D;
        border: none;
        color: rgb(50, 50, 50);
        text-align: center;
        text-decoration: none;
        font-size: 40px;
        cursor: pointer;
        border-radius: 10px;
    }
}

.user {
    position: relative;
    height: 100%;
    width: 30%;
    // float: right;
    flex: right;
    // border: red solid 1px;

    .d1 {
        position: relative;
        height: 100%;
        width: 30%;
        display: flex;
        justify-content: center;
        align-items: center;
        float: left;
        // border: red solid 1px;

        span {
            position: relative;
            font-size: 20px;
            color: rgb(10, 10, 10);
        }
    }

    .d2 {
        height: 100%;
        width: 20%;
        display: flex;
        justify-content: center;
        align-items: center;
        float: right;

        button {
            position: relative;
            background-color: #96B97D;
            border: none;
            color: rgb(50, 50, 51);
            text-align: center;
            text-decoration: none;
            font-size: 20px;
            cursor: pointer;
            border-radius: 10px;
        }
    }
}
</style>