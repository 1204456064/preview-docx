

<template>
    <div> 
      <input type="file" id="fileInput" @change="uploadChange">
      <div id="preview" v-html="vHtml"></div>
    </div>
  </template>
  <script setup lang="ts">
  import mammoth from 'mammoth'
  import { ref } from 'vue';
  
  /**
   * v-html
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
      // 读取完成，调用mammoth的convertToHtml函数获取解析到的数据
      mammoth.convertToHtml({arrayBuffer: reader.result as ArrayBuffer}).then((resultObject)=>{
        vHtml.value = resultObject.value
      })
    }
  }
    
  </script>
  