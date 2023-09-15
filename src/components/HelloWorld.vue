

<template>
  <div> 
    <input type="file" id="fileInput" @change="uploadChange">
    <div id="preview"></div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import $ from 'jQuery'
import '/js/pptxjs'
/**
 * 文件上传
 */
function uploadChange(){
//   $('#preview')[0].innerHTML = 'dddd'
// return
  $("#preview").pptxToHtml({
	pptxFileUrl: "Sample_12.pptx",
	fileInputId: "fileInput",
	slideMode: false,
	keyBoardShortCut: false,
	slideModeConfig: {  //on slide mode (slideMode: true)
	    first: 1, 
	    nav: false, /** true,false : show or not nav buttons*/
	    navTxtColor: "white", /** color */
	    navNextTxt:"&#8250;", //">"
	    navPrevTxt: "&#8249;", //"<"
	    showPlayPauseBtn: false,/** true,false */
	    keyBoardShortCut: false, /** true,false */
	    showSlideNum: false, /** true,false */
	    showTotalSlideNum: false, /** true,false */
	    autoSlide: false, /** false or seconds (the pause time between slides) , F8 to active(keyBoardShortCut: true) */
	    randomAutoSlide: false, /** true,false ,autoSlide:true */ 
	    loop: false,  /** true,false */
	    background: "black", /** false or color*/
	    transition: "default", /** transition type: "slid","fade","default","random" , to show transition efects :transitionTime > 0.5 */
	    transitionTime: 1 /** transition time in seconds */
    }
  });
  return
  // 读取上传后的文件
  const file= document.getElementById('fileInput').files[0]

  // 创建FileReader实例
  const reader = new FileReader()

  // 指定读取的内容是ArrayBuffer类型的数据
  reader.readAsArrayBuffer(file)

  reader.onload = () => {
  }
}

</script> 