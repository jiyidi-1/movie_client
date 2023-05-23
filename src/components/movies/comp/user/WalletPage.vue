<template>
    <div class="body">
        <div class="title">
            <span style="font-size: 40px;font-style: #a0c089;">钱包</span>
        </div>
        <br><br>
        <div>
            <div class="d1">
                <span>当前余额: {{user.money}}</span>
            </div>
            <br>
            <div class="d2">
                <input type="text" v-model="amount"/>
                <button @click="recharge">充值</button>
            </div>
            <div class="" v-if="stat == 1" >
                <h3>恭喜您，充值成功</h3>
            </div>
            <div class="" v-if="stat == 2" >
                <h3>请填入充值额度</h3>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'WalletPage',
    props: ['user'],
    data() {
        return {
            amount: '',
            stat: 0
        }
    },
    methods: {
        recharge(){
            if(this.amount == ""){
                this.stat = 2;
                setTimeout(() => {
                    this.stat = 0
                }, 2000);
                return
            }

            // 修改后端余额数据
            this.axios.put('/wallet/' + this.user.id + '/recharge/' + this.amount, {
                balance: this.amount
            }, {
                headers: {
                    token: this.user.token
                }
            }).then(res => {
                // console.log("recharge@", res, res.data.data.balance)
                if(res.data.message == "充值成功"){
                    // 显示充钱提示两秒
                    this.stat = 1;
                    setTimeout(() => {
                        this.stat = 0
                    }, 2000);

                    this.$bus.$emit('recharge', res.data.data.balance)
                }else{
                    alert("充值失败")
                }
            })
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