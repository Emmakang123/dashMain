<script setup>

const regester = function() {
    console.log('등록하기 이동')
}
</script>
<template>
    <div class="eventbox-wrapper">
        <span>이벤트</span> 
        <button type="button" class="addbtn btn btn-light" @click="regester">등록</button>
        <!-- <div v-for="(item, idx) in 3">
            <span>이벤트명</span><button>예약현황</button>
            <span>예약자 수 </span>
        </div> -->
    </div>
</template>
<style>
    .eventbox-wrapper{
        padding: 1rem;
    }
    .addbtn {
        margin-left: 1.5rem;
    }
</style>