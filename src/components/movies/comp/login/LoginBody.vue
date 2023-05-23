<template>
    <div>
        <div class="s">
            <h1>Hello Buddy, let's get it!</h1>
            <LoginUser v-if="funcNum == 0"  :user="user" :login="login"></LoginUser>
            <RegUser v-if="funcNum == 1"></RegUser>
        </div>
    </div>
</template>

<script>
import LoginUser from './LoginUser';
import RegUser from './RegUser';


export default {
    name: 'LoginBody',
    props: ['funcNum', 'user', 'login'],
    components: {
        LoginUser,
        RegUser
    }
}
</script>

<style scoped>
.s {
    position: absolute;
    width: 100%;
    height: 100%;
    text-align: center;
}
</style>