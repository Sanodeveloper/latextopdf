<script setup lang="ts">
import type { TextInfo } from '@/interfaces.ts'
const mapData = useState<Map<number, TextInfo>>('text');
let idNum = useState<number>('idnum');


const headText = ref('');
const dataText = ref('');
const row = ref();
const cell = ref();
const cap = ref('');

const onAddheader = (text: string): void => {
    const thead = document.getElementById('thead') as HTMLTableElement;
    const tbody = document.getElementById('tbody') as HTMLTableElement;
    const lineLen = tbody.rows.length;
    const tr = document.getElementById('table_header');
    const th = document.createElement('th');
    th.textContent = text;
    tr?.append(th);
    for (let i = 0; i < lineLen; i++) {
        const tr = tbody.children[i] as HTMLTableRowElement;
        tr.insertCell(-1);
    }

}

const onAddData = (text: string, row: number, cell: number): void => {
    const thead = document.getElementById('thead') as HTMLTableElement;
    const tbody = document.getElementById('tbody') as HTMLTableElement;

    let tr;
    if(row == 0) {
        tr = thead.children[row] as HTMLTableRowElement;
    } else {
        tr = tbody.children[row - 1] as HTMLTableRowElement;
    }
    const td = tr.children[cell - 1];
    td.textContent = text;
}

const onAddRow = (): void => {
    const table = document.getElementById('table') as HTMLTableElement;
    const tbody = document.getElementById('tbody') as HTMLTableElement;
    const cellLen = table.rows[0].cells.length;
    const tr = tbody.insertRow();
    for (let i = 0; i < cellLen; i++) {
        const td = document.createElement('td');
        td.textContent = '';
        tr.append(td);
    }

}

const onDeleCell = (cell: number): void => {
    const thead = document.getElementById('thead') as HTMLTableElement;
    const tbody = document.getElementById('tbody') as HTMLTableElement;
    const rowLen = tbody.rows.length;
    thead.rows[0].deleteCell(cell-1);
    for(let i = 0; i < rowLen; i++) {
        tbody.rows[i].deleteCell(cell-1);
    }
}

const onDeleRow = (row: number): void => {
    if (row != undefined) {
        const tbody = document.getElementById('tbody') as HTMLTableElement;
        tbody.deleteRow(row - 1);
    }
}

const onAddTable = ():void => {
    const table = document.getElementById('table') as HTMLTableElement;
    const thead = document.getElementById('thead') as HTMLTableElement;
    const tbody = document.getElementById('tbody') as HTMLTableElement;
    let array: Array<any> = new Array(table.rows.length);
    for(let i = 0; i < table.rows.length; i++) {
        array[i] = new Array(table.rows[0].cells.length);
    }
    let text;
    let type;
    for(let i = 0; i < table.rows.length; i++) {
        for(let j = 0; j < table.rows[0].cells.length; j++) {
            if(i == 0) {
                const th = thead.children[i].children[j];
                text = th.textContent != undefined ? th.textContent : '';
                type = 'th';
            } else {
                const td = tbody.children[i-1].children[j];
                text = td.textContent != undefined ? td.textContent : '';
                type = 'td';
            }
            array[i][j] = { type: type, text: text };
        }
    }
    mapData.value.set(idNum.value, {role: 'table', type: 'table', text: cap.value, data: array});
    idNum.value++;
}
</script>

<template>
    <div class="inner">
        <table id="table" border="2" cellpadding="10">
            <thead id="thead">
                <tr id="table_header">
                </tr>
            </thead>
            <tbody id="tbody">
            </tbody>
        </table>
        <div class="input">
            <div class="addinput">
                <div class="adddata">
                    <input v-model="headText" type="text" placeholder="題名を入力">
                    <button v-on:click="onAddheader(headText)">題名を追加</button>
                </div>
                <div class="adddata">
                    <label for="">行を追加</label>
                    <button v-on:click="onAddRow">追加</button>
                </div>
                <div class="adddata data">
                    <input v-model="dataText" type="text" placeholder="データを入力">
                    <input v-model.number="row" type="text" placeholder="行">
                    <input v-model.number="cell" type="text" placeholder="列">
                    <button v-on:click="onAddData(dataText, row, cell)">追加・変更</button>
                </div>
            </div>
            <div class="deleteinput">
                <div class="adddata">
                    <input v-model.number="row" type="text" placeholder="行">
                    <button v-on:click="onDeleRow(row)">行を削除</button>
                </div>
                <div class="adddata">
                    <input v-model.number="cell" type="text" placeholder="列">
                    <button v-on:click="onDeleCell(cell)">列を削除</button>
                </div>
            </div>
            <button class="addTable" v-on:click="onAddTable" style="margin-right: 10px;">テーブルを追加</button>
            <input v-model="cap" type="text" placeholder="キャプション">
        </div>
    </div>
</template>

<style scoped>
.inner {
    max-width: 100%;
    display: flex;
    color: #fff;
    background-color: #333333;
    overflow-x: auto;
}

table {
    min-width: 20%;
    margin: 0 20px;
}

.addinput,
.deleteinput {
    padding: 10px;
    border: 1px solid #fff;
}

.adddata {
    margin-bottom: 30px;
}

.adddata>input {
    margin-right: 10px;
    padding: 5px;
}

label {
    margin-right: 10px;
}

.data>input {
    display: block;
}

.addTable {
    margin: 10px 0;
}
</style>