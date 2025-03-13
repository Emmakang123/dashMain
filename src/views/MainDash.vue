<script setup>
import { onMounted, ref , computed, watchEffect, watch} from 'vue';

const eventsPerPage = 5; // 한 번에 보여줄 카드 개수
const currentPage = ref(0); // 현재 페이지 번호
const paginatedEvents = ref([]);
const isLoaded = ref(false);
const today = new Date().toISOString().split("T")[0];

const eventData = ref([]);
const fetchEventData = async () => {
    try {
        const res = await fetch('/src/assets/site-test.json');
        const data = await res.json();
        eventData.value = data.event;
        isLoaded.value= true;
    } catch (error) {
        console.log("error : ", error)
    }
}

const filteredEvents = computed( () => 
    eventData.value.filter(event => event.date >= today)
    .sort( (a,b ) => new Date(a.date) - new Date(b.date))
    .slice(0,5)
)

// 최대 페이지 계산
const maxPage = computed(() => filteredEvents.value.length > 0
    ? Math.ceil(filteredEvents.value.length / eventsPerPage) - 1
    : 0);

// 페이지별 데이터 계산 (페이지 변경 시 자동 반영)
const updatePaginatedEvents = () => {
  const start = currentPage.value * eventsPerPage;
  paginatedEvents.value = filteredEvents.value.slice(start, start + eventsPerPage);
};

watchEffect(updatePaginatedEvents, currentPage);

const prevPage = () => {
    if (currentPage.value > 0) currentPage.value--;
}
const nextPage = () => {
    if (maxPage.value > 0 && currentPage.value < maxPage.value) {
        currentPage.value++;
        updatePaginatedEvents(); 
    }
}



onMounted(() => {
    fetchEventData();
    updatePaginatedEvents();

})

console.log("maxpage : ", maxPage.value, " , currentPage : ", currentPage.value)
</script>
<template>
    <div class="siteres-wrapper">
        <p>공간별 예약현황</p>
        <div class="card-wrapper"> 
             <!-- 이전 버튼 (왼쪽) -->
            <button @click="prevPage" v-show="currentPage > 0" class="prev-btn">〈</button>

            <div class="card-container">
                <div v-for="event in filteredEvents" :key="event.date + event.site" class="site-res card mb-3" style="width: 10rem;">
                    <div class="card-body">
                        <h5 class="card-title">{{ event.site }}</h5>
                        <p class="card-text"><strong>이벤트:</strong> {{ event.type }}</p>
                        <p class="card-text"><strong>예약 수:</strong> {{ event["res-num"] }}명</p>
                        <p class="card-text" v-if="event['can-num'] > 0"><strong>취소 대기:</strong> {{ event["can-num"] }}건</p>
    
                        <a href="#" class="btn btn-primary">Go somewhere</a>
                    </div>
                </div>
            </div>
            <button @click="nextPage" v-show="maxPage >= currentPage" class="next-btn">〉</button>
        </div>
    </div>
</template>
<style>
.card-container{
    display: flex;    
}
.site-res{
    width: 10rem;
    margin: 0 1.25rem;
}
.card-wrapper{
    display: flex;
    align-items: center;
}
.siteres-wrapper{
    width: 100%;
}

@media screen and (max-width: 768px) {
    .card-container {
        gap: 0.5rem;
    }
    .site-res {
        /* width: 8rem;  */
        margin: 0 0.5rem;
    }
}

</style>