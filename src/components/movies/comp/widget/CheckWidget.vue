<template>
    <div class="body" ref="check_widget">
        <div class="upbody">
            <h2>座位情况</h2>
            <h3>已预订(红) {{arrow}} 未预定(黑)</h3>
        </div>
        
        <div class="inbody">
            <div class="seat_graph" v-for="(s, index) of seats" :key="index">
                <div v-if="s == 0" class="seat_empty" @click="buy(index + 1)">{{index + 1}}</div>
                <div v-else class="seat_full">{{index + 1}}</div>
            </div>
        </div>

        <div style="position:relative;top: 27%;"><button @click="cancel">完成</button></div>
    </div>
</template>

<script>
export default {
    name: 'CheckWidget',
    data(){
        return {
            cinema : {},
            schedule : [],
            seats : [],
            arrow : "<=>"
        }
    },
    props: ['user'],
    methods: {
        cancel(){
            this.$refs.check_widget.style.display = "none";
        },
        buy(seat_num){
            if(!confirm("是否购买此座位"))return

            // 进行购买
            var buy_info = {
                sid: this.schedule.id,
                seat: seat_num,
                userid: this.user.id
            }

            this.axios.post('/order/ticket', buy_info, {
                headers:{
                    token: this.user.token
                }
            }).then(res => {
                console.log(res)
                if(res.data.message == "购买成功"){
                    // 购票后更新余额
                    this.axios.get('/wallet/' + this.user.id, {
                        headers: {
                            token: this.user.token
                        }
                    }).then(res => {
                        if(res.data.message == "获取余额成功"){
                            this.$bus.$emit("buy_success_money", res.data.data.balance)
                        }
                    })
                    
                // 获取座位信息
                this.axios.get('/order/schedule/' + this.schedule.id + '/seat', {
                        headers : {
                            token: this.user.token
                        }
                    }).then(res => {
                        // console.log("seat_res", res)
                        if(res.data.message == "座位查询已完成"){
                            // 初始化座位信息
                            this.seats = []
                            for(let i = 0; i < this.schedule.seat_num; i++){
                                this.seats.push(0)
                            }
                            // 
                            // console.log("res.data.data.seat.length():", res.data.data.seat.length)
                            for(let i = 0; i < res.data.data.seat.length; i++){
                                this.seats[res.data.data.seat[i] - 1] = 1
                            }
                            console.log("seats", this.seats)
                        }else{
                            alert("座位查询失败")
                        }
                    })
                }else{
                    alert("购票失败")
                }
            })
        }
    },
    mounted(){
        this.$bus.$on("check_widget", (cinema, schedule) => {
            // 查看用户是否登录
            if(this.user.stat == 0){
                alert("请先登录")
                return
            }

            this.$refs.check_widget.style.display = "flex";

            // 获取影院电影列表信息
            this.cinema = cinema
            this.schedule = schedule

            // 获取座位信息
            this.axios.get('/order/schedule/' + this.schedule.id + '/seat', {
                headers : {
                    token: this.user.token
                }
            }).then(res => {
                // console.log("seat_res", res)
                if(res.data.message == "座位查询已完成"){

                    // 初始化座位信息
                    this.seats = []
                    for(let i = 0; i < this.schedule.seat_num; i++){
                        this.seats.push(0)
                    }
                    // 
                    // console.log("res.data.data.seat.length():", res.data.data.seat.length)
                    for(let i = 0; i < res.data.data.seat.length; i++){
                        this.seats[res.data.data.seat[i] - 1] = 1
                    }
                    console.log("seats", this.seats)
                }else{
                    alert("座位查询失败")
                }
            })
        })

        this.$refs.check_widget.style.display = "none";
    },
    beforeDestroy(){
        this.$bus.$off("check_widget");
    }
}
</script>

<style scoped lang="less">
.body{
    position: fixed;
    width: 50%;
    height: 50%;
    top: 20%;
    left: 25%;
    background-color: white;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.upbody{
    position: relative;
    top: -30%;
    height: 20%;

    /* border: black solid 2px; */
}

.inbody{
    position: relative;
    top: -20%;
    height: 15%;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;

    // border: black solid 2px;

    .seat_graph{
        // border: pink solid 2px;
        display: flex;

        .seat_empty{
            position: relative;
            background-color: black;
            width: 40px;
            height: 40px;
            border: black solid 1px;
            
            margin-left: 10px;
            margin-right: 10px;
            margin-bottom: 20px;

            color: white;

            cursor: pointer;
        }

        .seat_full{
            position: relative;
            background-color: red;
            width: 40px;
            height: 40px;
            border: black solid 1px;

            margin-left: 10px;
            margin-right: 10px;
            margin-bottom: 20px;

            color: white;
        }
    }

}
</style>