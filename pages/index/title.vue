<script setup lang="ts">
import type { TextInfo } from '@/interfaces.ts'
const mapData = useState<Map<number, TextInfo>>('text');
let idNum = useState<number>('idnum');

const title = ref('');
const auther = ref('');
const date = ref('');

const onAddTitle = (): void => {
    mapData.value.set(idNum.value, { role: "title", type: "h1", text: title.value, data:[]});
    idNum.value++;
    mapData.value.set(idNum.value, { role: "auther", type: "h1", text: auther.value, data:[]});
    idNum.value++;
    mapData.value.set(idNum.value, { role: "date", type: "h1", text: date.value, data:[]});
    idNum.value++;
}
</script>

<template>
    <div class="inner">
        <div>
            <label class="title" for="title">タイトル</label>
            <input v-model="title" type="text" id="title">
        </div>
        <div>
            <label class="auther" for="auther">著者</label>
            <input v-model="auther" type="text" id="auther">
        </div>
        <div>
            <label class="date" for="date">日付</label>
            <input v-model="date" type="text" id="date">

        </div>
        <button v-on:click="onAddTitle">反映</button>
    </div>
</template>

<style scoped>
.inner {
    height: 100%;
    background-color: #333333;
    color: #fff;
}

div {
    text-align: center;
    margin-bottom: 20px;
}

input {
    display: inline-block;
    width: 300px;
    padding: 5px 10px;
    border-radius: 4px;
    margin: 0 auto;
}

button {
    width: 100px;
}

.title {
    margin-right: 10px;
}

.auther {
    margin-right: 40px;
}

.date {
    margin-right: 40px;
}
</style>