

<template>
  <div> 
    <input type="file" id="fileInput" @change="uploadChange">
    <div v-html="vHtml"></div>
  </div>
</template>
<script setup lang="ts">
import * as XLSL from 'xlsx'
import { ref } from 'vue';
/**
 *  v-html
 */
const vHtml = ref<string>('')

/**
 * 文件上传
 */
function uploadChange(){
  // 读取上传后的文件
  const file= document.getElementById('fileInput').files[0]

  // 创建FileReader实例
  const reader = new FileReader()

  // 指定读取的内容是ArrayBuffer类型的数据
  reader.readAsArrayBuffer(file)

  reader.onload = () => {
    // 读取完成，调用XLSL的read函数获取解析上传的xlsx
    let workbook =  XLSL.read(reader.result,{type:'array'})
    // 一般都只有一个sheet,取第一个sheet的名称，方便取到sheet的数据
    let sheetNames = workbook.SheetNames[0]

    // 根据sheet的名称获取sheet数据解析成html
    let html  = XLSL.utils.sheet_to_html(workbook.Sheets[sheetNames])
    
    // 将渲染容器替换成解析出来的html标签渲染
    vHtml.value =  html
  }
}


</script>
<!-- <style>
table {
    border: 1px solid black;
}

th {
    border: 1px solid black;
}
td {
    border: 1px solid black;
}
</style> -->