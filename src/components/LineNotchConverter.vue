<script setup>
import { ref, computed, watch } from 'vue';

let reelText = ref(`1	1	1	1	1
2	2	2	2	2
0	0	0	0	0
3	3	3	3	3
0	1	2	1	0
3	2	1	2	3
2	1	0	1	2
1	2	3	2	1
0	1	0	1	0
3	2	3	2	3
2	1	2	1	2
1	2	1	2	1
1	1	0	1	1
2	2	3	2	2
0	1	1	1	0
3	2	2	2	3
1	0	0	0	1
2	3	3	3	2
0	0	1	0	0
3	3	2	3	3`);
let reelResult = ref('');
let reelRows = ref(0);
let reelCols = ref(0);

function process() {
  let result = '';

  // Clean input
  reelText.value = reelText.value.trim();

  // Get starting data row and count cols
  let rows = reelText.value.split('\n');
  let cols = 0;
  let rowStart = 0;
  for (let i = 0; i < rows.length; i++) {
    let n = rows[i].trim().split('\t').length;
    if (n > 1) {
      cols = n;
      rowStart = i;
      break;
    }
  }

  // Save reel size
  this.reelRows.value = rows;
  this.reelCols.value = cols;

  // Grab data column by column
  for (let j = 0; j < cols; j++) {
    let colSymbols = [];
    for (let i = rowStart; i < rows.length; i++) {
      let sym = rows[i].split('\t')[j];
      if (sym.length == 0) break;
      colSymbols.push(sym);
    }
    result += `REEL#${j + 1}[${colSymbols.length}] = ${JSON.stringify(
      colSymbols
    )}\n\n`;
  }

  reelResult.value = result;
}

watch([reelText], (newValue, oldValue) => {
  process();
});

process();
</script>

<template>
  <b>INPUT (from Excel tab separated):</b>
  <textarea v-model="reelText" style="width: 100%" rows="20" />
  <b>OUTPUT ({{ reelCols }}x{{ reelRows }}):</b>
  <textarea v-model="reelResult" style="width: 100%" rows="15" />
</template>

<style scoped></style>
