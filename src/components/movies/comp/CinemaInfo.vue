<template>
    <div class="body">
        <div class="title">
            <div><span style="font-size: 40px;font-style: #a0c089;float: left;">{{this.cinemaInfo.name}}影院</span></div>
            <div><span style="font-size: 20px;">地址：{{this.cinemaInfo.city}}  {{this.cinemaInfo.loc}}</span></div>
        </div>
        <div class="title2">
        <span style="font-size: 50px;font-style: #a0c089;float: left;">正在上映：</span>
        </div>
        <div class="lists">
            <div class="list" v-for="(schedule, index) of schedules" :key="index">
                <div class="movie_title">
                    <span style="font-size: 30px;float: left;">电影：{{ schedule.movieName }}</span>
                    <span>上映日期 {{schedule.time}}</span>
                </div>
                <div class="movie_body">
                    <span style="font-size: 20px;float: left;">影厅：{{ schedule.hall_Num }}号厅</span>
                    <span style="font-size: 20px;float: left;">价格：{{ schedule.price }}元</span>
                    <span v-if="user.auth == 1" style="font-size: 20px;float: left;">盈利：{{ schedule.profit }}元</span>
                    <div>
                        <button v-if="user.auth == 0 && user.stat == 1" @click="check_widget(index)">查看座位</button>
                        <button v-if="user.auth == 0 && user.stat == 1" @click="buy_widget(index)">订票</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    name: 'CinemaInfo',
    data() {
        return {
            cinemaInfo: {},
            schedules: []
        }
    },
    methods: {
        check_widget(index){
            this.$bus.$emit("check_widget", this.cinemaInfo, this.schedules[index]);
        },
        buy_widget(index){
            this.$bus.$emit("buy_widget", this.cinemaInfo, this.schedules[index]);
        }
    },
    props: ['cinemaList', 'user'],
    mounted(){
        // 绑定进入影院事件以取得所需要的影院信息
        this.$bus.$on("get_into_cinema", (cinema) => {
            // console.log("get_into_movie", cinema);
            this.cinemaInfo = cinema;

        })
    },
    beforeDestroy(){
        this.$bus.$off("get_into_cinema");
    },
    watch: {
        // 由于页面跳转的方式是改变这里，这里的cinemaInfo改变相当于页面展示，我们在这里做update时需要的操作
        cinemaInfo: {
            deep: true,
            handler(){
                // console.log("watch cinemaInfo");

                // 获取影院电影列表信息
                var url = "/theater/schedule/info/" + this.cinemaInfo.name;
                this.axios.get(url, {
                    headers : {
                        token : this.user.token
                    }
                }).then(res => {
                    console.log(res)
                    if(res.data.message == this.cinemaInfo.name + "的排片信息"){
                        this.schedules = [];
                        for(let i = 0; i < res.data.data.schedules.length; i++){
                            this.schedules.push(res.data.data.schedules[i]);
                        }
                        console.log("schedules is ", this.schedules);
                    }else{
                        // alert("获取排片信息失败");
                    }
                })
            }
        }
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
    display : flex;
    justify-content: space-between;
    align-items: center;
    border-bottom: black solid 2px;
}

.title2{
    border-bottom: black solid 2px;
}
.list {
    border-top: black solid 2px;
    height: 150px;
    vertical-align: middle;

    .movie_title{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }

    .movie_body{
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
}
</style>