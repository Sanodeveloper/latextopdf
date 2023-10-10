<script setup lang="ts">
import type { TextInfo } from '@/interfaces.ts'
const mapData = useState<Map<number, TextInfo>>('text');
let idNum = useState<number>('idnum');

const cap = ref('');
let path = ref('');
const onAddImage = (path: string):void => {
    mapData.value.set(idNum.value, {role: 'image', type: 'div', text: cap.value, data: [path]});
    idNum.value++;
}
</script>

<template>
    <div class="inner">
        <input type="text" v-model="path" placeholder="画像パスを入力">
        <button v-on:click="onAddImage(path)">追加</button>
        <p>画像パスを相対パスか絶対パスで入力してください。</p>
        <p>例:./image.png</p>
        <input type="text" v-model="cap" placeholder="キャプション">
    </div>
</template>

<style scoped>
.inner {
    width: 50%;
    margin: 0 auto;
    margin-top: 80px;
}

input {
    margin-right: 10px;
}

p {
    color: #fff;
}
</style>