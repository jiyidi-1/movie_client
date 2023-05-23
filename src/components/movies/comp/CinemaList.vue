<template>
    <div class="body">
        <div class="title">
            <span style="font-size: 40px;font-style: #a0c089;float: left;">影院列表：</span>
        </div>
        <div>
            <div class="list" v-for="(c, index) of cinemaList" :key="index" >
                <span class="cinematext" @click="get_into_cinema(c)">电影院:{{ c.name }}  id:{{ c.id }}</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'CinemaList',
    data() {
        return {
        }
    },
    methods: {
        get_into_cinema(cinema){
            this.$bus.$emit('get_into_cinema', cinema)
            this.MainList(3)
        }
    },
    props: ['MainList', 'cinemaList'],
}
</script>

<style scoped lang="less">
.body {
    position: fixed;
    width: 98%;
    height: 90%;
    top: 10%;
    left: 1%;
    background-color: #c2cbbd;
    display: flex;
    flex-flow: column;
    // justify-content: center;
    overflow-y: scroll;
}

.title{
    border-bottom: black solid 2px;
}

.list {
    border-top: black solid 2px;
    height: 150px;
    vertical-align: middle;

    .cinematext {
        font-size: 30px;
        float: left;
        cursor: pointer;
    }
}
</style>