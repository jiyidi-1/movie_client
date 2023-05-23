<template>
    <div class="body" ref="buy_widget">
        <div class="upbody"><h2>请输入购票信息</h2></div>
        <h3 ref="s_m">购票成功，即将自动关闭</h3>
        <div class="inbody">
            <input type="text" placeholder="请输入座位" v-model="seat_num">
            <button @click="buy">确认购买</button>
            <button @click="cancel">取消</button>
        </div>
    </div>
</template>

<script>
export default {
    name: 'BuyWidget',
    data(){
        return {
            seat_num: "",
            cinema : {},
            schedule : [],
        }
    },
    props: ['user'],
    methods: {
        buy(){
            if(this.seat_num == ""){
                alert("请输入座位号")
                return
            }

            if(this.user.stat == 0){
                alert("请先登录")
                return
            }

            if(this.user.auth == 1){
                alert("只有用户可以购票")
                return
            }

            var buy_info = {
                sid: this.schedule.id,
                seat: this.seat_num,
                userid: this.user.id
            }

            this.axios.post('/order/ticket', buy_info, {
                headers:{
                    token: this.user.token
                }
            }).then(res => {
                console.log("buy", res)
                if(res.data.message == "购买成功"){
                    // 显示购票成功提示
                    this.$refs.s_m.style.display = "inline";

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
                    

                    // 两秒后关闭购票成功提示
                    setTimeout(() => {
                        this.$refs.s_m.style.display = "none";
                        this.$refs.buy_widget.style.display = "none";
                    }, 2000);
                }else{
                    alert("购票失败")
                }
            })
        },
        cancel(){
            this.$refs.buy_widget.style.display = "none";
        }
    },
    mounted(){
        this.$bus.$on("buy_widget", (cinema, schedule) => {
            console.log("buy_widget", cinema, schedule);
            this.$refs.buy_widget.style.display = "flex";

            // 获取影院电影列表信息
            this.cinema = cinema
            this.schedule = schedule
        })

        this.$refs.s_m.style.display = "none";
        this.$refs.buy_widget.style.display = "none";
    },
    beforeDestroy(){
        this.$bus.$off("buy_widget");
    }
}
</script>

<style scoped>
.body{
    position: fixed;
    width: 40%;
    height: 40%;
    top: 20%;
    left: 30%;
    background-color: white;
    display: flex;
    flex-direction: column;
    justify-content: center;
}

.upbody{
    position: relative;
    top: -10%;
    height: 20%;

    /* border: black solid 2px; */
}

.inbody{
    position: relative;
    top: 0%;
    height: 15%;
    display: flex;
    justify-content: center;

    /* border: black solid 2px; */
}
</style>