<template>
    <div>
        <h2>请输入您想更改的上映信息:</h2>
        <input type="text" v-model="theater_name" placeholder="请输入影院名" />
        <br/><br/>
        <input type="text" v-model="movie_name" placeholder="请输入电影" />
        <br/><br/>
        请选择上映期:  <input type="date" v-model="stime.date"/><input type="time" v-model="stime.time"/>
        <br/><br/>
        <input type="text" v-model="hall_num" placeholder="请输入所在大厅" />
        <br/><br/>
        <input type="text" v-model="price" placeholder="请输入电影价格" />
        <br/><br/>
        <input type="text" v-model="seat_num" placeholder="请输入座位数" />
        <br/><br/>
        请选择电影状态：
        待放映<input type="radio" v-model="status" value="0" />
        已放映<input type="radio" v-model="status" value="1" />
        <br/><br/>
        <button @click="addCinema">添加</button>
    </div>
</template>

<script>

export default {
    name: 'ChangeSchedule',
    data() {
        // console.log(this)
        return {
            theater_name : "",
            movie_name : "",
            stime : {
                date : "2023-01-01",
                time : "00:00"
            },
            hall_num : "",
            price : "",
            seat_num : "",
            status : "0"
        }
    },
    computed: {
        stime_total () {
            console.log(this.stime)
            return this.stime.date + " " + this.stime.time
        }
    },
    props: ['user', 'login'],
    methods: {
        addCinema(){
            if(this.theater_name == "" || this.movie_name == "" || this.hall_num == "" || this.price == "" || this.seat_num == "" || this.status == ""){
                alert("请填写完整信息")
                return
            }

            var schedule = {
                "theaterName": this.theater_name,
                "movieName": this.movie_name,
                "time" : this.stime_total,
                "hall_Num" : this.hall_num,
                "price" : this.price,
                "seat_num" : this.seat_num,
                "status" : this.status
            }

            console.log("schedule", schedule)
            this.axios.post("/theater/schedule/arrange", schedule, {
                headers : {
                    token : this.user.token
                }
            }).then(res => {
                console.log(res)
                if(res.data.message == "排片成功"){
                    alert("添加成功")
                }
                else{
                    alert("添加失败")
                }
            })
        }
    }
}
</script>