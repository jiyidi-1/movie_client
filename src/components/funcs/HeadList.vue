<template>
    <div>
        <div class="choose" v-for="(f, index) of funcs" :key="index" @click="change(index)">
            <button class="but">{{f}}</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'HeadList',
    data() {
        return {
            funcs: [
                "增加用户",
                "查询用户",
                "查询全部",
                "修改用户",
                "删除用户"
            ]
        }
    },
    props: ['funcNum', 'changeF'],
    methods: {
        change(index) {
            console.log("change in", index);
            this.changeF(index);
        }
    }
}
</script>

<style scoped>
.choose {
    display: inline-block;
    position: relative;
    height: 100%;
    width: 18%;
    margin-left: 1%;
    margin-right: 1%;

    /* border-style: solid;
    border-color: purple; */
}

.but {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    height: 80%;
    width: 100%;

    /* background-color: antiquewhite; */
    /* border-style: hidden; */
    background-color: #96B97D;
    border: none;
    color: white;
    text-align: center;
    text-decoration: none;
    font-size: 20px;
    cursor: pointer;
    border-radius: 10px;
}
</style>