<script setup lang="ts">
import type { TextInfo } from '@/interfaces.ts'
const mapData = useState<Map<number, TextInfo>>('text');
let idNum = useState<number>('idnum');

const subSection = ref('');

const onAddTitle = (): void => {
    mapData.value.set(idNum.value, {role: "subsection", type: "h2", text: subSection.value, data:[] });
    idNum.value++;
}
</script>

<template>
    <div class="inner">
        <div class="input">
            <label class="subsection" for="subsection">見出し(中)</label>
            <input v-model="subSection" type="text" id="subsection">
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

.subsection {
    margin-right: 40px;
}
</style>