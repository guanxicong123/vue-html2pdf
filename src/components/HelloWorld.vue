<template>
  <div id="PdfPage1">
      <button type="button" class="btn btn-primary" @click="pdfBtn">导出PDF</button>
      <input type="file" @change="inputFile">
      <div id="pdfDom" style="padding:10px;background-color:#fff;" class="pdfDom">
        <div class="name-text">{{name}}</div>
        <img src="../../关11.png" class="picture"/>
      </div>
  </div>
</template>

<script setup>
import { nextTick, ref } from "vue";
  import htmlToPdf from '../html2pdf'
  import {fileToJson} from '../handleExcel'
  import excelData from '../../excel.json'
  const htmlTitle = '11'
  console.log(excelData,'excelData');
  const pdfBtn = ()=>{
    if(currentLength < excelDataLength){
      name.value = excelData[currentLength][0]
      nextTick(()=>{
        htmlToPdf.getPdf(document.querySelector('#pdfDom'),htmlTitle,()=>{
          currentLength++
          pdfBtn()
        });
      })
    }
    
  }
  const inputFile = (e)=>{
    console.log(e.target.files[0],'file');
    fileToJson(e.target.files[0],(sheets)=>{
      console.log(sheets,'sheets');
    })
  }
  const name = ref('陈老板')
  let currentLength = 1
  const excelDataLength = excelData.length
  const downloadPdf = ()=>{
    pdfBtn()
  }
  nextTick(()=>{
    downloadPdf()
  })
  // const readExcel = ()=>{
  //   readWorkbookFromLocalFile('C:/Users/guanxc/Desktop/test/excelVue/excel2pdf/src/e.xlsx')
  // }
  setTimeout(()=>{
    // readExcel()
  })
</script>

<style scoped>
.pdfDom{
  height: 595px;
  width: 841px;
  background-image: url('../pdfbg.jpg');
  background: url('../pdfbg.jpg') 100% 100% no-repeat;
  background-size: 100% 100%;

  position: relative;
}
.name-text{
  position: absolute;
  left: 510px;
  top: 211px;
  font-weight: 700;
}
.picture{
  position: absolute;
  width: 100px;
  top: 70px;
  left: 536px;
}
</style>