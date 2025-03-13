<script setup>
    import { computed, onMounted, ref } from 'vue';
    import {Bar} from 'vue-chartjs'

    import {
    Chart as ChartJS,
    Title,
    Tooltip,
    Legend,
    CategoryScale,
    LinearScale,
    PointElement,
    Filler,
    BarElement
    } from "chart.js";

    ChartJS.register(Filler, Title, Tooltip, Legend, BarElement, CategoryScale, LinearScale, PointElement);

    const fetchEventData = ref({
        "event" : [
        {
            "date": "2025-03-09",
            "site": "소공연장",
            "type": "공연",
            "eventName" : "taylor Swift Tour",
            "resMeth": "온라인예약",
            "resNum": 494,
            "canNum": 27,
            "realNum": 467
        },
        {
            "date": "2025-03-09",
            "site": "대강당",
            "eventName" : "workshop",
            "type": "공연",
            "resMeth": "현장예약",
            "resNum": 462,
            "canNum": 10,
            "realNum": 452
        },
        {
            "date": "2025-03-24",
            "site": "대강당",
            "type": "공연",
            "eventName" : "IU World Tour / Orange",
            "resMeth": "온라인예약",
            "resNum": 455,
            "canNum": 13,
            "realNum": 442
        }
        ],
        "totalNum" : 10484,
    });

const rateCal = computed( () => {
    return fetchEventData.value.event.map( event => (
        {
            ...event, 
            rate : ((event.realNum/ fetchEventData.value.totalNum) *100).toFixed()
        }
    ))
})

const getChartData = event => ({
    labels : [event.rate + '%'],
    datasets: [
        {
            label : '실제 참여자 수',
            data : [event.realNum],
            backgroundColor : ['#4CAF50']
        }
    ]
})
onMounted( () => {
    // mount할떄 동작
    console.log(rateCal.value)
})

const chartOptions = {
    responsive : true,
    maintainAspectRatio: false,
    indexAxis: "y",
    scales: {
        x: {
            min: 0, 
            max: fetchEventData.value.totalNum, 
            ticks: {
                display: true 
            },
            grid: {
                drawTicks: false, 
                drawBorder: false 
            }
        },
        y: {
            ticks: {
                font: {
                    size: 12
                }
            },
            grid: {
                drawBorder: false
            }
        }
    },
    plugins: {
        legend: {
            display: false
        },
  }
}
</script>
<template>
    <div class="top-wrapper">
        <p>인기 이벤트</p>
        <div class="top-container">
            <div class="event-item" v-for="(event, idx) in rateCal">
                <span class="rank">{{ idx + 1 }}. </span>
                <span class="event-name">{{ event.site }} - {{ event.eventName}} ({{ event.type }})</span>
                <span class="count">{{ event.realNum}}명</span>
                <div class="chart-container" >
                    <Bar :data="getChartData(event)" :options="chartOptions"></Bar>
                </div>
            </div>

        </div>
    </div>
</template>
<style>
.top-wrapper{
    border: 1px solid #d2d2d2;
    border-radius: 15px;
    padding: 8px;
}

.top-container{
    display: flex;
    flex-direction: column;
    gap: 10px;

}
.event-item {
    display: flex;
    align-items: center; 
    justify-content: space-between;
    gap: 15px;
}

.rank {
    font-size: 1.2rem;
    font-weight: bold;
    color: #ff5722;
}

.chart-container {
    width: 50%; 
    height: 40px;
    margin-left: auto;
}
</style>