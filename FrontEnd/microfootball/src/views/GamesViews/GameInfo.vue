<template>
    <el-icon @click="back" style="margin-top: 30px; margin-left: 50px;">
        <Back />
    </el-icon>
    <div style="display: flex;flex-direction: column; align-items: center;margin-top: 50px;">
        <div style="display: flex;">
            <p style="font-size: xxx-large; font-weight: bold; color: #ff575780;">{{ leagueName }}</p>
        </div>
        <div style="width: 900px; display: flex;flex-direction: column;margin-top: 20px;">
            <p style=" font-size: x-large; font-weight: bold; color: #043c8aa6;display: flex;justify-content: center;">{{
                year }}-{{ month }}-{{ day }}&nbsp&nbsp{{ hour }}:{{ minute }}:{{ second }}</p>
            <div style="display: flex;justify-content: space-between;padding: 30px;">
                <div style="width: 30%;display: flex;flex-direction: column;align-items: center;"
                    @click="toteaminfo(homeTeamId)">
                    <div><img :src="homeTeamLogo" /></div>
                    <div class="teamText">{{ homeTeamName }}</div>
                </div>
                <div
                    style="width: 40%;display: flex;justify-content: center; align-items: center; font-size: xxx-large; font-weight: bolder;">
                    <p>{{ homeGoal }} : {{ awayGoal }}</p>
                </div>
                <div style="width: 30%;display: flex;flex-direction: column;align-items: center;"
                    @click="toteaminfo(homeTeamId)">
                    <div><img :src="awayTeamLogo" /></div>
                    <div class="teamText">{{ awayTeamName }}</div>
                </div>
            </div>
        </div>
        <div class="match-status">
            <a-button type="primary" danger v-if="statusEndYes">Match Finshed</a-button>
            <a-button type="primary" v-if="statusEndNot" @click="Follow">Follow</a-button>
        </div>
    </div>
    <!-- <div class="league-logo">
        <img :src="leagueLogo">
    </div>
    <div class="match-name">
        <p style="font-size: 40px; padding:10px color:yellow">{{ leagueName }}</p>
        <p style="font-size: 25px; padding:10px;">{{ round }}</p>
    </div>

    <div class="match-info">
        <img :src="homeTeamLogo">
        <p>{{ homeGoal }} - {{ awayGoal }}</p>
        <img :src="awayTeamLogo">
    </div>

    <div class="match-team">
        <p>{{ homeTeamName }}</p>
        <p>{{ awayTeamName }}</p>
    </div>

    <div class="match-status">
        <a-button type="primary" danger v-if="statusEndYes">Match Finshed</a-button>
        <a-button type="primary" v-if="statusEndNot" @click="Follow">Follow</a-button>
    </div>

    <div class="match-place">
        <p>{{ venueCity }} {{ venueName }}</p>
        <p>{{ date }}</p>
    </div> -->
</template>

<script setup>
import { useRouter, useRoute } from 'vue-router';
import { ref, onMounted } from 'vue'
import axios from 'axios'
import { ElMessage } from 'element-plus'
console.log('hello')

const router = useRouter();
const route = useRoute()
console.log('route', route)
const gameId = route.params.id;


console.log('hhh')

const leagueLogo = ref('') // leaguelogo
const leagueName = ref('')
const round = ref('')
const homeTeamLogo = ref('')
const homeGoal = ref()
const awayGoal = ref()
const awayTeamLogo = ref('')
const homeTeamName = ref('')
const awayTeamName = ref('')
const venueCity = ref('')
const venueName = ref('')
const date = ref('')
const status = ref('')
const statusEndYes = ref(true)
const statusEndNot = ref(false)

let year = ref('')
let month = ref('')
let day = ref('')
let hour = ref('')
let minute = ref('')
let second = ref('')

onMounted(async () => {
    loadGame()
})
const back = () => {
    router.back();
}
function convertToBeijingTime(isoString) {
    console.log(isoString)
    const utcDate = new Date(isoString);
    console.log(utcDate)
    const beijingTimeOffset = 8 * 60; // 北京时间比UTC时间快8小时
    utcDate.setMinutes(utcDate.getMinutes() + beijingTimeOffset);

    year.value = utcDate.getFullYear().toString();
    month.value = (utcDate.getMonth() + 1).toString().padStart(2, '0'); // getMonth() 返回的月份是从0开始的
    day.value = utcDate.getDate().toString().padStart(2, '0');
    hour.value = utcDate.getHours().toString().padStart(2, '0');
    minute.value = utcDate.getMinutes().toString().padStart(2, '0');
    second.value = utcDate.getSeconds().toString().padStart(2, '0');
}

const loadGame = async () => {
    const res = await axios.get(`/api/game/detail?id=${gameId}`)
    // const res = {"code":1,"msg":"success","data":{"id":1037664,"date":"2023-08-13T10:40:00+00:00","status":"Match Finished","venueName":"Polsat Plus Arena Gdańsk","venueCity":"Gdańsk","homeTeamId":343,"homeTeamName":"Lechia Gdansk","homeTeamLogo":"https://media-4.api-sports.io/football/teams/343.png","awayTeamId":6964,"awayTeamName":"Znicz Pruszków","awayTeamLogo":"https://media-4.api-sports.io/football/teams/6964.png","homeGoal":1,"awayGoal":0,"leagueName":"I Liga","leagueLogo":"https://media-4.api-sports.io/football/leagues/107.png","round":"Regular Season - 4"}}
    const data = res.data.data
    // console.log(data)
    leagueLogo.value = data.leagueLogo
    leagueName.value = data.leagueName
    round.value = data.round
    homeTeamLogo.value = data.homeTeamLogo
    homeGoal.value = data.homeGoal
    awayGoal.value = data.awayGoal
    awayTeamLogo.value = data.awayTeamLogo
    venueCity.value = data.venueCity
    venueName.value = data.venueName
    date.value = data.date
    homeTeamName.value = data.homeTeamName
    awayTeamName.value = data.awayTeamName
    status.value = data.status
    // console.log('gameId', gameId)
    // console.log(awayTeamLogo.value)
    convertToBeijingTime(date.value)
    if (status.value != 'Match Finished') {
        statusEndYes.value = false
        statusEndNot.value = true
    }
}

const toteaminfo = (id) => {
    router.push({
        path: `/team/${id}`
    })
}

const Follow = async () => {
    const token = localStorage.getItem('token')
    const response = await axios.post(`/api/game/subscribe-game?gameId=${gameId}&startTime=${date.value}`, {}, {
        headers: {
            'token': token,
        }
    })
    if (response.data.meg == 'subscribeError') {
        ElMessage({
            message: '订阅失败',
            type: 'error'
        })
    }
    else {
        ElMessage({
            message: '订阅成功',
            type: 'success'
        })
    }
}

</script>


<style scoped>
.league-logo {
    display: grid;
    place-items: center;
    /* 水平垂直居中 */
    padding: 25px
}

.match-name {
    padding: 25px;
    display: grid;
    place-items: center;
    /* 水平垂直居中 */
    font-weight: bolder;
}

.match-info {
    display: grid;
    grid-template-columns: auto 1fr auto;
    /* 三个列，自动调整宽度 */
    align-items: center;
    /* 垂直居中对齐 */
    gap: 10px;
    /* 列之间的间距，根据实际情况调整 */
}

.match-info img {
    width: 250px;
    /* 设置图片宽度，根据实际情况调整 */
    height: auto;
    /* 保持图片纵横比 */
}

.match-info p {
    text-align: center;
    font-weight: bolder;
    font-size: 80px
}

.match-team {
    display: flex;
    align-items: center;
    /* 居中对齐 */
    justify-content: space-between;
    /* 左右间距平分空间 */
}

.match-team p {
    text-align: center;
    font-weight: bolder;
    font-size: 30px;
    margin: 0;
    /* 清除段落的默认外边距，确保它们之间没有间距 */
}

.match-place {

    text-align: center;
    font-weight: light;
    font-size: 20px;
    padding: 20px;

}

.teamText {
    font-size: x-large;
    font-weight: bolder;
}

.match-status {
    display: grid;
    place-items: center;
    /* 水平垂直居中 */
    padding: 25px
}
</style>