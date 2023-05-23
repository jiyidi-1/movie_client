<template>
    <div>
        <h2>请输入所删除的用户ID:</h2>
        <input type="text" v-model="id" placeholder="请输入ID" />
        <button @click="deleteU">删除用户</button>
    </div>
</template>

<script>
export default {
    name: 'DeleteUser',
    data() {
        // console.log(this)
        return {
            id : 0,
            status : 0,
        }
    },
    methods: {
        deleteU() {
            this.axios.delete('/api/v1/user', {
                params:{
                    uid : this.id
                }
            }).then(res => {
                // console.log('post succeed')
                console.log(res)
                if (res.data == "Delete success.") {
                    alert("删除用户成功！")
                }else if(res.data == "There is no such user."){
                    alert("用户不存在！")
                }
            })
        }
    }
}
</script>