<script setup lang="ts">
import type { TextInfo } from '@/interfaces.ts'
const mapData = useState<Map<number, TextInfo>>('text');
let idNum = useState<number>('idnum');


let data = ref('');
let listType = ref('ul');

const onAddList = (type: string): void => {
    if(type == 'ul') {
        listType.value = 'ul';
    } else if(type == 'ol') {
        listType.value = 'ol'
    } else {
        listType.value = '';
    }

    const preview = document.getElementById('listPreview');
    const list = document.createElement(type);
    list.style.color = '#fff'
    preview?.append(list);
}

const onAddData = (text: string): void => {
    const preview = document.getElementById('listPreview');
    const list = preview?.children[0];
    const li = document.createElement('li');
    if (list != undefined) {
        li.textContent = text;
        list.append(li);
    }
}

const onDeleList = (): void => {
    const list = document.getElementById('listPreview');
    if (list != null) {
        for (let el of list?.children) {
            el.remove();
        }
    }
}

const onAddDatatoDerectry = ():void => {
    const preview = document.getElementById('listPreview');
    const li = preview?.children[0].children as HTMLCollection;
    let array: Array<any> = new Array(li?.length);

    for(let i = 0; i < array.length; i++) {
        array[i] = li[i].textContent;
    }

    mapData.value.set(idNum.value, {role: 'list', type: listType.value, text: 'list', data: array});
    idNum.value++;
}
</script>
<template>
    <div class="inner">
        <div class="preview" id="listPreview">

        </div>
        <div class="input">
            <button v-on:click="onAddList('ol')">番号付き</button>
            <button v-on:click="onAddList('ul')">点</button>
            <div class="adddata">
                <input v-model.number="data" type="text" placeholder="リストを入力">
                <button v-on:click="onAddData(data)">追加</button>
            </div>
            <button v-on:click="onDeleList">消去</button>
            <button id="addList" v-on:click="onAddDatatoDerectry">リストを追加</button>
        </div>
    </div>
</template>
<style scoped>
.inner {
    width: 100%;
    display: flex;
}

.preview {
    width: 50%;
    min-height: 100px;
    overflow-x: scroll;
}

.input {
    padding: 15px;
    width: 50%;
    border: 1px solid #fff;
}

.adddata {
    margin-bottom: 30px;
    margin-top: 30px;
}

#addList {
    margin-left: 10px;
}
</style>