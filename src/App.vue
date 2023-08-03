<script setup lang="ts">
import { ref } from 'vue'
import { emojify } from '@twuni/emojify'
// const { emojify } = require('@twuni/emojify')
console.log(emojify(''))
const formated = ref('')
const readCSVFile = () => {
  var files = document.querySelector('#file').files
  if (files.length > 0) {
    // Selected file
    var file = files[0]
    // FileReader Object
    var reader = new FileReader()

    // Read file as string
    reader.readAsText(file)

    // Load event
    reader.onload = function (event) {
      // Read file data
      var csvdata = event.target.result

      // Split by line break to gets rows Array
      var rowData = csvdata.split('\n')

      // Loop on the row Array (change row=0 if you also want to read 1st row)
      for (var row = 0; row < rowData.length; row++) {
        let rowColData = rowData[row].split(',')
        for (var col = 0; col < rowColData.length; col++) {
          // const transformedString = rowColData[col].length ? emojify(rowColData[col]) : ''
          const transformedString = emojify(rowColData[col])
          const temp = col == rowColData.length - 1 ? transformedString : transformedString + ','
          formated.value += temp
        }
        formated.value += row == rowData.length - 1 ? '' : '\n'
      }
      console.log(formated.value)
    }
  } else {
    alert('Please select a file.')
  }
}
</script>
<template>
  <div>
    <div>
      <input type="file" name="file" id="file" accept=".csv" /> <br /><br />
      <input type="button" id="btnsubmit" value="Submit" @click="readCSVFile" />
    </div>

    <br /><br />
    <!-- List CSV file data -->
    <table id="tblcsvdata" border="1" style="border-collapse: collapse">
      <thead></thead>
      <tbody></tbody>
    </table>
  </div>
</template>

<style scoped></style>
