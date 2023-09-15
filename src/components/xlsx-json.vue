

<template>
    <div> 
      <input type="file" id="fileInput" @change="uploadChange">
      <a-table :dataSource="dataSource" :columns="columns" bordered />  </div>
  </template>
  <script setup lang="ts">
  import * as XLSL from 'xlsx'
  import { ref } from 'vue';
  
  interface rowType {
    [key:string]: string
  }
  
  /**
   *  table columns
   */
  const columns = ref<rowType[]>([])
  
  
  /**
   *  table dataSource
   */
   const dataSource = ref<rowType[]>([])
  
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
      let jsonData  = XLSL.utils.sheet_to_json(workbook.Sheets[sheetNames]) as rowType[]

      // 去excel第一行当表头
      Object.keys(jsonData[0]).forEach((item: string)=>{
        columns.value.push({
          title: item,
          dataIndex: item
        })
      })
  
      // 其他行都当做数据
      jsonData.forEach((item: rowType)=>{
        let obj:rowType = {}
        
        Object.keys(item).map((key:string)=>{
          obj[key] = item[key]
        })
  
        dataSource.value.push(obj)
      })

    }
  }
  
  </script>