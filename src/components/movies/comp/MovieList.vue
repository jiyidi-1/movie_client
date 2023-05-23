<template>
    <div class="body">
        <div class="title">
            <span style="font-size: 40px;font-style: #a0c089;float: left;">电影列表：</span>
        </div>
        <div class="lists">
            <div class="list" v-for="(c, index) of movieList" :key="index">
                <span style="font-size: 30px;float: left;">电影：{{ c.name }}   id:{{ c.id }}</span>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'MovieList',
    data() {
        return {
            movieList: [
                { name: "电影1", data: 1, id: 1 }
            ]
        }
    },
    methods: {
    },
    mounted(){
        // console.log("CinemaList onShow");
        this.axios.get('/movie/all', {
        }).then(res => {
            console.log(res)
            if(res.data.message == "获取所有电影信息"){
                this.movieList.pop();
                for(let i = 0; i < res.data.data.movies.length; i++){
                    this.movieList.push(res.data.data.movies[i]);
                }
            }
        })
    },
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
}
</style>