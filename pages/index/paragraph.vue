<script setup lang="ts">
import type { TextInfo } from '@/interfaces.ts'
const mapData = useState<Map<number, TextInfo>>('text');
let idNum = useState<number>('idnum');

const para = ref('');

const onAddTitle = (): void => {
    mapData.value.set(idNum.value, {role: "para", type: "p", text: para.value, data:[] });
    idNum.value++;
}
</script>

<template>
    <div class="inner">
        <div class="input">
            <label class="para" for="para">文章</label>
            <input v-model="para" type="text" id="para">
            <p>改行したかったら新しい文章ブロックを追加してね</p>
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
    padding: 40px;
}

p {
    margin-top: 10px;
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

.para {
    margin-right: 40px;
}
</style>