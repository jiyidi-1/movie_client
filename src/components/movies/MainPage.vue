<template>
    <div class="main">
        <TopList :MainList="MainList" :AppPage="AppPage" :user="user" :logout="logout"></TopList>
        <CinemaList v-show="list == 1" :MainList="MainList" :cinemaList="cinemaList"></CinemaList>
        <MovieList v-show="list == 2"></MovieList>
        <CinemaInfo v-show="list == 3" :cinemaList="cinemaList" :user="user"></CinemaInfo>
        <WalletPage v-show="list == 4" :user="user"></WalletPage>
        <TicketPage v-show="list == 5" :user="user"></TicketPage>
        <BuyWidget :user="user"></BuyWidget>
        <CheckWidgetVue :user="user"></CheckWidgetVue>

    </div>
</template>

<script>
import TopList from './comp/TopList.vue';
import CinemaList from './comp/CinemaList.vue';
import MovieList from './comp/MovieList.vue';
import CinemaInfo from './comp/CinemaInfo.vue';
import WalletPage from './comp/user/WalletPage.vue'
import TicketPage from './comp/user/TicketPage.vue';
import BuyWidget from './comp/widget/BuyWidget.vue'
import CheckWidgetVue from './comp/widget/CheckWidget.vue'

export default {
    name: 'MainPage',
    components: {
        TopList,
        CinemaList,
        MovieList,
        CinemaInfo,
        WalletPage,
        TicketPage,
        BuyWidget,
        CheckWidgetVue
    },
    data() {
        return {
            list: (sessionStorage.getItem("list") == 3 ? 1 : sessionStorage.getItem("list")) || 1,
            cinemaList: [
                { name: "电影院1", address: "地址1", id: 1 },
            ]
        }
    },
    props: ['AppPage', 'user', 'logout'],
    methods: {
        MainList(index) {
            // console.log("changeList in", index);
            this.list = index;
            this.whichcinema = index;
        }
    },
    watch: {
        list: {
            handler: function (val) {
                sessionStorage.setItem("list", val);
            },
            deep: true
        }
    },
    mounted(){
        // console.log("CinemaList onShow");
        this.axios.get('/theater/all', {
        }).then(res => {
            console.log(res)
            if(res.data.message == "获取所有电影院信息"){
                this.cinemaList.pop();
                for(let i = 0; i < res.data.data.theaters.length; i++){
                    this.cinemaList.push(res.data.data.theaters[i]);
                }
            }
        })

        // 绑定改变主页页面事件
        this.$bus.$on('show_main_page',(list)=>{
            this.list = list;
        })
    },
    beforeDestroy() {
        this.$bus.$off('show_main_page')
    },
}

</script>

<style scoped lang="less">
.main{
    position: relative;
    width: 100%;
}


</style>