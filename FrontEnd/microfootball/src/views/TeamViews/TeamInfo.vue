<template>
    <!-- <div style="margin-top:-70px; width: 100%;">
        <div style="width: 100%;">
            <iframe style="width: 100%; height: 100vh;"
                src="https://cn.bing.com/sportsdetails?q=AC%E7%B1%B3%E5%85%B0&sport=Soccer&scenario=Team&TimezoneId=China%20Standard%20Time&defaultleague=Soccer_ItalySerieA&intent=Generic&team=SportRadar_Soccer_ItalyCoppaItalia_2023_Team_2692,SportRadar_Soccer_ItalySerieA_2023_Team_2692,SportRadar_Soccer_InternationalClubsClubFriendlyGames_2023_Team_2692,SportRadar_Soccer_InternationalClubsIntChampionsCup_2019_Team_2692,SportRadar_Soccer_UEFAEuropaLeague_2023_Team_2692,SportRadar_Soccer_InternationalClubsUEFAChampionsLeague_2023_Team_2692&segment=sports&isl2=true&"
                frameborder="0" allowfullscreen>
            </iframe>
        </div>
    </div> -->
    <el-icon @click="back" style="margin-top: 30px; margin-left: 50px;">
        <Back />
    </el-icon>
    <div class="photo">
        <img :src="logo">
    </div>
    <div class="baisc-info">
        <a-descriptions title="基本信息" bordered>
            <a-descriptions-item label="名称">{{ name }}</a-descriptions-item>
            <a-descriptions-item label="场馆">{{ venueName }}</a-descriptions-item>
            <a-descriptions-item label="地址">{{ venueAddress }}</a-descriptions-item>
            <a-descriptions-item label="城市">{{ venueCity }}</a-descriptions-item>
            <a-descriptions-item label='国家'>{{ country }}</a-descriptions-item>
            <a-descriptions-item label="建立时间">{{ founded }}</a-descriptions-item>
        </a-descriptions>
    </div>

    <div id="playercard">
        <MemberItem v-for="player in playerList" :key="player" :player="player" @click="toplayerInfo(player)" />
    </div>
</template>

<script setup>
import { useRoute, useRouter } from 'vue-router';
import { ref, onMounted } from 'vue'
import axios from 'axios'
import MemberItem from '@/components/PlayerComponents/MemberItem.vue'
const route = useRoute();
const router = useRouter();
// console.log('route', route)
const teamId = route.params.id;
const logo = ref('')
const name = ref('')
const founded = ref('')
const venueName = ref('')
const venueAddress = ref('')
const country = ref('')
const venueCity = ref('')
let playerList = ref([{
    "id": 0,
    "name": "",
    "age": 0,
    "number": 0,
    "position": "",
    "photo": ""
}])

const back = () => {
    router.back();
}
const columns = [
    {
        title: '姓名',
        dataIndex: 'name'
    },
    {
        title: '年龄',
        dataIndex: 'age',
    },
    {
        title: '编号',
        dataIndex: 'number'
    },
    {
        title: '位置',
        dataIndex: 'position',
    },
]

const toplayerInfo = (player) => {
    router.push({
        path: `/Players/${player.id}`
    })
}

onMounted(async () => {
    //初始化
    //获取数据
    //const res = {"code":1,"msg":"success","data":{"id":33,"name":"Manchester United","country":"England","logo":"https://media-4.api-sports.io/football/teams/33.png","founded":"1878","venueName":"Old Trafford","venueAddress":"Sir Matt Busby Way","venueCity":"Manchester","venueImage":"https://media-4.api-sports.io/football/venues/556.png","venueCapacity":76212,"players":[{"id":50132,"name":"A. Bayındır","age":25,"number":1,"position":"Goalkeeper","photo":"https://media-4.api-sports.io/football/players/50132.png"},{"id":2931,"name":"T. Heaton","age":37,"number":22,"position":"Goalkeeper","photo":"https://media-4.api-sports.io/football/players/2931.png"},{"id":526,"name":"A. Onana","age":27,"number":24,"position":"Goalkeeper","photo":"https://media-4.api-sports.io/football/players/526.png"},{"id":284361,"name":"R. Vítek","age":20,"number":40,"position":"Goalkeeper","photo":"https://media-4.api-sports.io/football/players/284361.png"},{"id":889,"name":"V. Lindelöf","age":29,"number":2,"position":"Defender","photo":"https://media-4.api-sports.io/football/players/889.png"},{"id":2935,"name":"H. Maguire","age":30,"number":5,"position":"Defender","photo":"https://media-4.api-sports.io/football/players/2935.png"},{"id":2467,"name":"Lisandro Martínez","age":25,"number":6,"position":"Defender","photo":"https://media-4.api-sports.io/football/players/2467.png"},{"id":37145,"name":"T. Malacia","age":24,"number":12,"position":"Defender","photo":"https://media-4.api-sports.io/football/players/37145.png"},{"id":739,"name":"Reguilón","age":27,"number":15,"position":"Defender","photo":"https://media-4.api-sports.io/football/players/739.png"},{"id":742,"name":"R. Varane","age":30,"number":19,"position":"Defender","photo":"https://media-4.api-sports.io/football/players/742.png"},{"id":886,"name":"Diogo Dalot","age":24,"number":20,"position":"Defender","photo":"https://media-4.api-sports.io/football/players/886.png"},{"id":891,"name":"L. Shaw","age":28,"number":23,"position":"Defender","photo":"https://media-4.api-sports.io/football/players/891.png"},{"id":18846,"name":"A. Wan-Bissaka","age":26,"number":29,"position":"Defender","photo":"https://media-4.api-sports.io/football/players/18846.png"},{"id":18772,"name":"J. Evans","age":35,"number":35,"position":"Defender","photo":"https://media-4.api-sports.io/football/players/18772.png"},{"id":288112,"name":"Willy Kambwala Ndengushi","age":18,"number":null,"position":"Defender","photo":"https://media-4.api-sports.io/football/players/288112.png"},{"id":74,"name":"S. Amrabat","age":27,"number":4,"position":"Midfielder","photo":"https://media-4.api-sports.io/football/players/74.png"},{"id":19220,"name":"M. Mount","age":24,"number":7,"position":"Midfielder","photo":"https://media-4.api-sports.io/football/players/19220.png"},{"id":1485,"name":"Bruno Fernandes","age":29,"number":8,"position":"Midfielder","photo":"https://media-4.api-sports.io/football/players/1485.png"},{"id":174,"name":"C. Eriksen","age":31,"number":14,"position":"Midfielder","photo":"https://media-4.api-sports.io/football/players/174.png"},{"id":747,"name":"Casemiro","age":31,"number":18,"position":"Midfielder","photo":"https://media-4.api-sports.io/football/players/747.png"},{"id":70078,"name":"F. Pellistri","age":22,"number":28,"position":"Midfielder","photo":"https://media-4.api-sports.io/football/players/70078.png"},{"id":547,"name":"D. van de Beek","age":26,"number":34,"position":"Midfielder","photo":"https://media-4.api-sports.io/football/players/547.png"},{"id":284322,"name":"K. Mainoo","age":18,"number":37,"position":"Midfielder","photo":"https://media-4.api-sports.io/football/players/284322.png"},{"id":903,"name":"S. McTominay","age":27,"number":39,"position":"Midfielder","photo":"https://media-4.api-sports.io/football/players/903.png"},{"id":303010,"name":"Daniel Gore","age":19,"number":44,"position":"Midfielder","photo":"https://media-4.api-sports.io/football/players/303010.png"},{"id":180560,"name":"H. Mejbri","age":20,"number":46,"position":"Midfielder","photo":"https://media-4.api-sports.io/football/players/180560.png"},{"id":284242,"name":"O. Forson","age":19,"number":null,"position":"Midfielder","photo":"https://media-4.api-sports.io/football/players/284242.png"},{"id":908,"name":"A. Martial","age":28,"number":9,"position":"Attacker","photo":"https://media-4.api-sports.io/football/players/908.png"},{"id":909,"name":"M. Rashford","age":26,"number":10,"position":"Attacker","photo":"https://media-4.api-sports.io/football/players/909.png"},{"id":288006,"name":"R. Højlund","age":20,"number":11,"position":"Attacker","photo":"https://media-4.api-sports.io/football/players/288006.png"},{"id":157997,"name":"A. Diallo","age":21,"number":16,"position":"Attacker","photo":"https://media-4.api-sports.io/football/players/157997.png"},{"id":284324,"name":"A. Garnacho","age":19,"number":17,"position":"Attacker","photo":"https://media-4.api-sports.io/football/players/284324.png"},{"id":9971,"name":"Antony","age":23,"number":21,"position":"Attacker","photo":"https://media-4.api-sports.io/football/players/9971.png"},{"id":18,"name":"J. Sancho","age":23,"number":25,"position":"Attacker","photo":"https://media-4.api-sports.io/football/players/18.png"},{"id":163054,"name":"S. Shoretire","age":19,"number":47,"position":"Attacker","photo":"https://media-4.api-sports.io/football/players/163054.png"},{"id":282133,"name":"J. Hugill","age":19,"number":52,"position":"Attacker","photo":"https://media-4.api-sports.io/football/players/282133.png"}]}}
    const res = await axios.get(`/api/team/detail?teamId=${teamId}`)
    const data = res.data.data
    console.log(data)
    logo.value = data.logo
    name.value = data.name
    founded.value = data.founded
    venueName.value = data.venueName
    venueAddress.value = data.venueAddress
    country.value = data.country
    venueCity.value = data.venueCity
    playerList.value = data.players
    console.log(playerList.value)
})


</script>


<style scoped>
.photo {
    display: grid;
    place-items: center;
    padding: 25px
}

#playercard {
    display: flex;
    justify-content: flex-start;
    /* 对齐到开始位置 */
    flex-wrap: wrap;
    /* 允许内容换行 */
    gap: 10px;
    /* 项目之间的间距 */
}

.baisc-info {
    display: grid;
    place-items: center;
    padding: 25px
}
</style>