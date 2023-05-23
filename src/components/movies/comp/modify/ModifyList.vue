<template>
    <div>
        <div class="choose" v-for="(f, index) of funcs" :key="index" @click="change(index)">
            <button class="but">{{f}}</button>
        </div>
        <div class="choose" @click="back">
            <button class="but">返回</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'ModifyList',
    data() {
        return {
            funcs: [
                "修改影院",
                "修改电影",
                "修改上映",
            ]
        }
    },
    props: ['funcNum', 'changeFunc', 'AppPage'],
    methods: {
        change(index) {
            this.changeFunc(index);
        },
        back(){
            this.AppPage(1);
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