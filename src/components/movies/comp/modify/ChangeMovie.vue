<template>
    <div>
        <h2>请输入您想更改的电影信息:</h2>
        <input type="text" v-model="id" placeholder="请输入电影id" />
        <br/><br/>
        <input type="text" v-model="name" placeholder="请输入电影名" />
        <br/><br/>
        <input type="text" v-model="actors" placeholder="请输入电影演员" />
        <br/><br/>
        <input type="text" v-model="duration" placeholder="请输入电影上映期" />
        <br/><br/>
        正在上映<input type="radio" v-model="status" value="1" />
        未上映<input type="radio" v-model="status" value="0" />
        已下架<input type="radio" v-model="status" value="2" />
        <br/><br/>
        显示<input type="radio" v-model="hide" value="1" />
        不显示<input type="radio" v-model="hide" value="2" />
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
            id : "",
            name : "",
            actors : "",
            duration : "",
            status : "1",
            hide : "1"
        }
    },
    props: ['user', 'login'],
    methods: {
        addCinema(){
            if(this.id == "" || this.name == "" || this.actors == "" || this.duration == "" || this.status == "" || this.hide == ""){
                alert("请填写完整信息")
                return
            }

            var movie = {
                name : this.name,
                actors : this.actors,
                duration : this.duration,
                status : this.status,
                hide : this.hide
            }
            this.axios.post('/movie/arrange', movie,{
                headers : {
                    "token" : this.user.token
                }
            }).then(res => {
                console.log(res)
                if(res.data.message == "创建新电影"){
                    alert("添加成功")
                }else{
                    alert("添加失败")
                }
            })
        },
        modifyCinema(){
            if(this.id == "" || this.name == "" || this.actors == "" || this.duration == "" || this.status == "" || this.hide == ""){
                alert("请填写完整信息")
                return
            }
            var movie = {
                id : this.id,
                name : this.name,
                actors : this.actors,
                duration : this.duration,
                status : this.status,
                hide : this.hide
            }
            this.axios.put('/movie/arrange', movie,{
                headers : {
                    "token" : this.user.token
                }
            }).then(res => {
                console.log(res)
                if(res.data.message == "修改电影数据"){
                    alert("修改成功")
                }else{
                    alert("修改失败")
                }
            })
        },
        deleteCinema(){
            if(this.id == ""){
                alert("请填写id信息")
                return
            }
            alert("等等再删")
        }
    }
}
</script>