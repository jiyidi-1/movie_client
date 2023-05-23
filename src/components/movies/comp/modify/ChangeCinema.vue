<template>
    <div>
        <h2>请输入您想更改的影院信息:</h2>
        <input type="text" v-model="id" placeholder="请输入影院id" />
        <br/><br/>
        <input type="text" v-model="name" placeholder="请输入影院名" />
        <br/><br/>
        <input type="text" v-model="city" placeholder="请输入影院城市" />
        <br/><br/>
        <input type="text" v-model="loc" placeholder="请输入影院地址" />
        <br/><br/>
        <input type="text" v-model="hallNumber" placeholder="请输入影院厅数" />
        <br/><br/>
        已开放<input type="radio" v-model="status" value="1" />
        未开放<input type="radio" v-model="status" value="0" />
        <br/><br/>
        <button @click="addCinema">添加</button>
        <button @click="modifyCinema">修改</button>
        <!-- <div v-if="status == 1">
            <h2>登陆成功</h2>
            <br><br>
            姓名：{{user.name}}
            <br><br>
            权限：{{user.auth ? "管理员" : "普通用户"}}
        </div>
        <div v-if="status == 2">
            <h2>请您输入正确的密码</h2>
        </div>
        <div v-if="status == 3">
            <h2>没有这个用户哦</h2>
        </div> -->
    </div>
</template>

<script>

export default {
    name: 'ChangeCinema',
    data() {
        // console.log(this)
        return {
            id: "",
            name : "",
            city : "",
            loc : "",
            hallNumber : "",
            status : "1"
        }
    },
    props: ['user'],
    methods: {
        addCinema(){
            if(this.id == "" || this.name == "" || this.city == "" || this.loc == "" || this.hallNumber == "" || this.status == ""){
                alert("请填写完整信息")
                return
            }
            var theater = {
                name : this.name,
                city : this.city,
                loc : this.loc,
                hallNumber : this.hallNumber,
                status : this.status
            }
            this.axios.post('/theater/arrange', theater,{
                headers: {
                    "token" : this.user.token
                }
            }).then(res => {
                console.log(res)
                if(res.data.message == "创建电影院"){
                    alert("添加成功")
                }else{
                    alert("添加失败")
                }
            })
        },
        modifyCinema(){
            if(this.id == "" || this.name == "" || this.city == "" || this.loc == "" || this.hallNumber == "" || this.status == ""){
                alert("请填写完整信息")
                return
            }
            var theater = {
                id: this.id,
                name : this.name,
                city : this.city,
                loc : this.loc,
                hallNumber : this.hallNumber,
                status : this.status
            }
            this.axios.put('/theater/arrange', theater, {
                headers: {
                    "token" : this.user.token
                }
            }).then(res => {
                if(res.data.message == "修改电影院数据"){
                    alert("修改成功")
                }else{
                    alert("修改失败")
                }
            })
        },
        deleteCinema(){
            // if(this.id == ""){
            //     alert("请填写id信息")
            //     return
            // }
            alert("电影院不可删除！")
        }
    }
}
</script>