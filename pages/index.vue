<script setup lang="ts">
import type { TextInfo } from '@/interfaces'
import { BrowserLocalStorage } from 'botbuilder';

//--------click------------------------------------------------------------------------------
const mapData = useState<Map<number, TextInfo>>('text');
let secNum = ref(0);
let subSecNum = ref(0);

const onChangeElem = () => {
    //---------Delete-----------------------
    const main = document.getElementById('preview');
    while (main?.firstChild) {
        main.removeChild(main.firstChild);
    }
    secNum.value = 0;
    subSecNum.value = 0;

    //-----------Add-------------------------
    const arrayData = Array.from(mapData.value.values());
    for (let i = 0; i < arrayData.length; i++) {
        const roleData = arrayData[i].role;
        const textData = arrayData[i].text;
        const typeData = arrayData[i].type;
        let elem = document.createElement(typeData);

        elem.style.fontWeight = '100';

        switch (roleData) {
            case 'section':
                subSecNum.value = 0;
                secNum.value++;
                elem.textContent = String(secNum.value) + '. ' + textData;
                main?.append(elem);
                break;
            case 'subsection':
                subSecNum.value++;
                elem.textContent = String(secNum.value) + '.' + String(subSecNum.value) + '. ' + textData;
                elem.style.marginLeft = '40px';
                main?.append(elem);
                break;
            case 'title':
                elem.textContent = textData;
                elem.style.textAlign = 'center';
                main?.append(elem);
                break;
            case 'auther':
                elem.textContent = textData;
                elem.style.textAlign = 'center';
                main?.append(elem);
                break;
            case 'date':
                elem.textContent = textData;
                elem.style.textAlign = 'center';
                main?.append(elem);
                break;
            case 'para':
                elem.textContent = textData;
                main?.append(elem);
                if (elem.previousElementSibling?.tagName === 'H1') {
                    elem.style.marginLeft = '50px';
                } else if (elem.previousElementSibling?.tagName === 'H2') {
                    elem.style.marginLeft = '100px';
                } else if (elem.previousElementSibling?.tagName === 'P') {
                    const element = elem.previousElementSibling
                    const style = window.getComputedStyle(element);
                    const value = style.getPropertyValue('margin-left');
                    elem.style.marginLeft = value;
                }
                break;
            case 'table':
                for (let j = 0; j < arrayData[i].data.length; j++) {
                    const tr = document.createElement('tr');
                    for (let k = 0; k < arrayData[i].data[0].length; k++) {
                        const tagType = document.createElement(arrayData[i].data[j][k].type);
                        tagType.textContent = arrayData[i].data[j][k].text;
                        tagType.style.border = '1px solid #333';
                        tr.append(tagType);
                    }
                    elem.append(tr);
                }
                elem.style.border = '1px solid #333';
                elem.style.margin = 'auto';
                main?.append(elem);
                break;
            case 'list':
                for (let j = 0; j < arrayData[i].data.length; j++) {
                    const li = document.createElement('li');
                    li.textContent = arrayData[i].data[j];
                    elem.append(li);
                }
                main?.append(elem);
                if (elem.previousElementSibling?.tagName === 'H1') {
                    elem.style.marginLeft = '50px';
                } else if (elem.previousElementSibling?.tagName === 'H2') {
                    elem.style.marginLeft = '100px';
                } else if (elem.previousElementSibling?.tagName === 'P') {
                    const element = elem.previousElementSibling
                    const style = window.getComputedStyle(element);
                    const value = style.getPropertyValue('margin-left');
                    elem.style.marginLeft = value;
                }
                break;
            case 'image':
                elem.style.width = '400px';
                elem.style.height = '200px';
                elem.style.border = '1px solid #333';
                elem.style.textAlign = 'center';
                elem.textContent = textData;
                main?.append(elem);
                break;
            default:
                elem.textContent = textData;
                break;
        }
    }
}

const onChangeLatex = (): void => {
    const arrayData = Array.from(mapData.value.values());
    let text = ref(['']);
    text.value = ['\\documentclass[dvipdfmx]{jsarticle}', '\\usepackage{graphicx}'];
    for (let i = 0; i < arrayData.length; i++) {
        const roleData = arrayData[i].role;
        const textData = arrayData[i].text;
        switch (roleData) {
            case 'section':
                text.value.push('\\section{' + textData + '}');
                break;
            case 'subsection':
                text.value.push('\\subsection{' + textData + '}');
                break;
            case 'title':
                text.value.push('\\title{' + textData + '}');
                break;
            case 'auther':
                text.value.push('\\author{' + textData + '}');
                break;
            case 'date':
                text.value.push('\\date{' + textData + '}');
                text.value.push('\\begin{document}');
                text.value.push('\\maketitle');
                break;
            case 'para':
                text.value.push(textData+'\\\\');
                break;
            case 'table':
                text.value.push('\\begin{table}[htbp]');
                text.value.push('\\centering');
                text.value.push('\\caption{' + textData + '}');
                const t: Array<string> = [];
                for (let j = 0; j < arrayData[i].data[0].length; j++) {
                    t.push('c|');
                }
                text.value.push('\\begin{tabular}{|' + t.join('') + '}\\hline');
                for (let j = 0; j < arrayData[i].data.length; j++) {
                    const t: Array<string> = [];
                    for (let k = 0; k < arrayData[i].data[0].length; k++) {
                        t.push(arrayData[i].data[j][k].text);
                    }
                    text.value.push(t.join('&') + '\\\\' + '\\hline');
                }
                text.value.push('\\end{tabular}');
                text.value.push('\\end{table}');
                break;
            case 'list':
                if (arrayData[i].type === 'ul') {
                    text.value.push('\\begin{itemize}');
                    for (let j = 0; j < arrayData[i].data.length; j++) {
                        text.value.push('\\item ' + arrayData[i].data[j]);
                    }
                    text.value.push('\\end{itemize}');
                } else {
                    text.value.push('\\begin{enumerate}');
                    for (let j = 0; j < arrayData[i].data.length; j++) {
                        text.value.push('\\item ' + arrayData[i].data[j]);
                    }
                    text.value.push('\\end{enumerate}');
                }
                break;
            case 'image':
                text.value.push('\\begin{figure}');
                text.value.push('\\centering');
                text.value.push('\\includegraphics[width=90mm]{' + arrayData[i].data[0] + '}');
                text.value.push('\\caption{' + textData + '}');
                text.value.push('\\end{figure}');
                break;
            default:
                break;
        }
    }
    text.value.push('\\end{document}');
    const blob = new Blob([text.value.join('\n')], { type: "text/plain" });
    const link = document.createElement('a');
    link.href = URL.createObjectURL(blob);
    link.download = 'report.tex';
    link.click();
}

const onDeleteElem = (id: number) => {
    mapData.value.delete(id);
    const main = document.getElementById('preview');
    while (main?.firstChild) {
        main.removeChild(main.firstChild);
    }
    onChangeElem();
}

</script>

<template>
    <div class="inner">
        <main id="preview" class="preview">

        </main>
        <div class="directory">
            <button v-on:click="onChangeElem" style="margin-right: 10px;">プレビュー</button>
            <button v-on:click="onChangeLatex">書き出し</button>
            <template v-for="[id, value] in mapData" v-bind:key="id">
                <div>
                    <p v-on:click="onDeleteElem(id)" class="text">{{ value.role }}:{{ value.text }}</p>
                </div>
            </template>
        </div>
    </div>
    <div class="inner">
        <div class="write">
            <NuxtPage />
        </div>
        <div class="aside">
            <NuxtLink v-bind:to="{ name: 'index-title' }">タイトル</NuxtLink>
            <NuxtLink v-bind:to="{ name: 'index-section' }">見出し(大)</NuxtLink>
            <NuxtLink v-bind:to="{ name: 'index-subSection' }">見出し(中)</NuxtLink>
            <NuxtLink v-bind:to="{ name: 'index-paragraph' }">文章</NuxtLink>
            <NuxtLink v-bind:to="{ name: 'index-table' }">表</NuxtLink>
            <NuxtLink v-bind:to="{ name: 'index-list' }">箇条書き</NuxtLink>
            <NuxtLink v-bind:to="{ name: 'index-image' }">画像・グラフ</NuxtLink>
        </div>
    </div>
</template>

<style scoped>
.inner {
    display: flex;
}

/** preview */

.preview {
    width: 80%;
    height: 70vh;
    border: 5px solid #2b2b2b;
    overflow: scroll;
    padding: 0 120px;
    background-color: #fff;
}


/** directory */

.directory {
    width: 20%;
    height: 70vh;
    border: 5px solid #2b2b2b;
    background-color: #333333;
    color: #fff;
    overflow: scroll;
}

.text {
    border-bottom: 1px solid #cccc;
    margin-bottom: 10px;
}

.text:hover {
    opacity: .8;
}


/** write */

.write {
    width: 70%;
    min-height: 30%;
    border: 5px solid #2b2b2b;
    background-color: #333333;
    border-bottom: none;
}


/** aside */

.aside {
    display: flex;
    flex-wrap: wrap;
    justify-content: flex-start;
    width: 30%;
    min-height: 30%;
    border: 5px solid #2b2b2b;
    background-color: #333333;
    border-bottom: none;
}

a {
    display: inline-block;
    width: 22%;
    max-height: 40px;
    color: #333333;
    text-decoration: none;
    border: 1px solid #fff;
    background-color: #fff;
    padding: 5px 15px;
    border-radius: 6px;
    margin-left: 10px;
    margin-bottom: 10px;
}

a:hover {
    opacity: .8;
}

button {
    border: 1px solid #fff;
    background-color: #fff;
    padding: 5px 15px;
    border-radius: 6px;
    margin-top: 5px;
    margin-bottom: 10px;
}

button:hover {
    opacity: .8;
}
</style>