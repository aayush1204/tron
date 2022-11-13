<template>
  <div class="container">
    <h1>SurveyJS Library / Runner</h1>
    <Survey :survey="survey" />
  </div>
</template>

<script>
import { Survey, Model, StylesManager } from "survey-vue";
import "survey-vue/defaultV2.css";
StylesManager.applyTheme("defaultV2");
import { json } from "../data/survey_json";


import {  setLibraryContract, getTronWeb, makeSurvey } from "../plugins/utils"
import { sampleTx } from "../plugins/walletConnect"
// import { Veu } from

// import JsonCSV from 'vue-json-csv'

// Vue.component('downloadCsv', JsonCSV)


export default {
  components: {
    Survey
  },
  
  data() {
    const model = new Model(json);
    model.onComplete.add(this.surveyComplete);
    return {
      survey: model
    };
  },
  methods: {
    surveyComplete (sender) {

     getTronWeb();
    
    // Wait a while to ensure tronweb object has already injected
    setTimeout(async ()=>{
        // init contract object
        await setLibraryContract();
        
        console.log("Begin to obtain the books information");
        // fetch all books
        // const books = await fetchAllBooks();
          
          const results = JSON.stringify(sender.data);
          alert(results);



         // <download-csv
           // :data = sender.data
           // name = file.csv>
          //  Download Data
         // </download-csv>

         await makeSurvey();
        // this.posts = books;
        // console.log("The total number of Books: "+ books.length);
        
    },50);
      
    },
    async sendTx() {
      await sampleTx()
    },
  },
};
</script>