<script setup lang="ts">
import type { TextInfo } from '@/interfaces.ts'
const mapData = useState<Map<number, TextInfo>>('text');
let idNum = useState<number>('idnum');

const section = ref('');

const onAddTitle = (): void => {
    mapData.value.set(idNum.value, {role: "section", type: "h1", text: section.value, data:[] });
    idNum.value++;
}
</script>

<template>
    <div class="inner">
        <div class="input">
            <label class="section" for="section">見出し(大)</label>
            <input v-model="section" type="text" id="section">
        </div>
        <button v-on:click="onAddTitle">反映</button>
    </div>
</template>

<style scoped>
.inner {
    text-align: center;
    height: 100%;
    background-color: #333333;
    color: #fff;
}

.input {
    margin-bottom: 20px;
    padding: 40px;
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

.section {
    margin-right: 40px;
}
</style>