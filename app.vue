<script setup lang="ts">
import type { TextInfo } from '@/interfaces';
useState<Map<number, TextInfo>>(
    "text",
    (): Map<number, TextInfo> => {
        const textMap = new Map<number, TextInfo>();
        return textMap;
    }
);

useState<number>(
  "idnum",
  (): number => {
    const idNum = 1;
    return idNum;
  }
);
</script>
<template>
  <div>
    <NuxtPage />
  </div>
</template>

<style>
body {
  box-sizing: border-box;
  margin: 0;
  background-color: #333333;
}
h1,h2,p,div {
  margin: 0;
}

h1 {
  margin-top: 10px;
}

td,
th {
    width: 80px;
    min-width: 80px;
    height: 60px;
    min-height: 60px;
}
</style>
