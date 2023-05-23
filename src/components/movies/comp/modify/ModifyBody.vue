<template>
    <div>
        <div class="s">
            <h1>Hello Buddy, let's get it!</h1>
            <ChangeCinema v-if="funcNum == 0"  :user="user"></ChangeCinema>
            <ChangeMovie v-if="funcNum == 1" :user="user"></ChangeMovie>
            <ChangeSchedule v-if="funcNum == 2" :user="user"></ChangeSchedule>
        </div>
    </div>
</template>

<script>
import ChangeCinema from './ChangeCinema';
import ChangeMovie from './ChangeMovie';
import ChangeSchedule from './ChangeSchedule';


export default {
    name: 'ModifyBody',
    props: ['funcNum', 'user'],
    components: {
        ChangeCinema,
        ChangeMovie,
        ChangeSchedule

    }
}
</script>

<style scoped>
.s {
    position: absolute;
    width: 100%;
    height: 100%;
    text-align: center;
}
</style>