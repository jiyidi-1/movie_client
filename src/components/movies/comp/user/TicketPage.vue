<template>
    <div class="body">
        <div class="title">
            <div><span style="font-size: 40px;font-style: #a0c089;float: left;">订单信息</span></div>
            <div><span style="font-size: 20px;">用户：{{this.user.name}}</span></div>
            <button v-if="user.auth == 0" @click="query_tickets(user.id)">查询订单</button>
            <div v-if="user.auth == 1">
                <input type="text" v-model="query_id" placeholder="请输入查询用户id">
                <button @click="query_tickets(query_id)">查询订单</button>
            </div>
        </div>
        <div class="title2">
        <span style="font-size: 50px;font-style: #a0c089;float: left;">历史订单：</span>
        </div>
        <div class="lists">
            <div class="list" v-for="(ticket, index) of tickets" :key="index">
                <div class="movie_title">
                    <span style="font-size: 30px;float: left;">电影：{{ schedules[index].movieName }}</span>
                    <span>放映日期 {{ schedules[index].time }}</span>
                </div>
                <div class="movie_body">
                    <span style="font-size: 20px;float: left;">座位：{{ ticket.seat }}</span>
                    <span style="font-size: 20px;float: left;">订单状态：{{ get_stat_string(ticket) }}</span>
                    <div>
                        <button v-if="ticket.status == 0 && user.auth == 0" @click="refund(ticket, schedules[index])">退票</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'TicketPage',
    data() {
        return {
            tickets: [],
            schedules: [],
            query_id: ""
        }
    },
    methods: {
        get_stat_string(ticket){
            if(ticket.status == 0){
                return "待使用";
            }else if(ticket.status == 1){
                return "已使用";
            }else if(ticket.status == 2){
                return "已退票";
            }
        },
        refund(ticket, schedule){
            this.axios.put("/order/refund/" + ticket.orderid, {},{
                headers : {
                    token : this.user.token
                }
            }).then(res => {
                console.log(res)
                if(res.data.message == "退票成功"){
                    alert("退票成功");
                    this.query_tickets(this.user.id);
                    this.$bus.$emit("recharge", this.user.money + schedule.price * 0.95);
                }else{
                    alert("退票失败");
                }
            })
        },
        query_tickets(query_id){
            if(query_id == ""){
                alert("请输入查询用户id");
                return;
            }

            // 获取电影票信息
            var url = "/order/user/" + query_id;
            this.axios.get(url, {
                headers : {
                    token : this.user.token
                }
            }).then(res => {
                console.log(res)
                if(res.data.message == "获取用户的所有订单信息"){
                    this.tickets = [];
                    for(let i = 0; i < res.data.data.tickets.length; i++){
                        this.tickets.push(res.data.data.tickets[i]);
                    }
                    console.log("tickets is ", this.tickets);

                    this.schedules = [];
                    for(let i = 0; i < res.data.data.schedules.length; i++){
                        this.schedules.push(res.data.data.schedules[i]);
                    }
                    console.log("schedules is ", this.schedules);
                }else{
                    alert("获取订单信息失败");
                }
            })
        }
    },
    props: ['user'],
    mounted(){
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