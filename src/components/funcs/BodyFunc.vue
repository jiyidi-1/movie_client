<template>
    <div>
        <div class="s">
            <h1>Hello Buddy, let's get it!</h1>
            <AddUser v-if="funcNum == 0"></AddUser>
            <FindUser v-if="funcNum == 1"></FindUser>
            <FindAll v-if="funcNum == 2"></FindAll>
            <ModifyUser v-if="funcNum == 3"></ModifyUser>
            <DEleteUser v-if="funcNum == 4"></DEleteUser>
        </div>
    </div>
</template>

<script>
import AddUser from './AddUser';
import FindUser from './FindUser';
import FindAll from './FindAll';
import ModifyUser from './ModifyUser';
import DEleteUser from './DEleteUser';

export default {
    name: 'BodyFunc',
    props: ['funcNum'],
    components: {
        AddUser,
        FindUser,
        FindAll,
        ModifyUser,
        DEleteUser
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