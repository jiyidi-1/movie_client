<template>
    <div id="app">
        <div class="ui">
            <div class="picture">
                <img alt="Vue logo" src="@/assets/logo.png" style="height: 100%;background-size: cover;" />
            </div>

            <div class="func">
                <LoginList class="head" :funcNum="funcNum" :changeFunc="changeFunc" :AppPage="AppPage"></LoginList>
                <LoginBody class="body" :funcNum="funcNum" :user="user" :login="login"></LoginBody>
            </div>
        </div>
    </div>
</template>
  
<script>
import LoginList from '@/components/movies/comp/login/LoginList.vue';
import LoginBody from '@/components/movies/comp/login/LoginBody.vue';

export default {
    name: 'LoginPage',
    components: {
        LoginList,
        LoginBody
    },
    data() {
        return {
            funcNum: [0]
        }
    },
    props: ['AppPage', 'user', 'login'],
    methods: {
        changeFunc(index) {
            this.funcNum.pop();
            this.funcNum.push(index);
        }
    }
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

.picture {
    display: inline-block;
    width: 38%;
    height: 100%;
    vertical-align: middle;
}

.func {
    display: inline-block;
    width: 62%;
}

.head {
    position: absolute;
    height: 20%;
    width: 60%;
    top: 0%;

    /* border-style: solid;
    border-color: red; */
    /* background-color: red; */
}

.body {
    position: absolute;
    height: 80%;
    width: 60%;
    top: 20%;

    /* border-style: solid;
    border-color: red; */
    /* background-color: yellow; */
}
</style>
  