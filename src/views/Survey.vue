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


import {  setLibraryContract, getTronWeb, test, makeSurvey } from "../plugins/utils"
import { sampleTx } from "../plugins/walletConnect"

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

          let jsonParsedString = JSON.parse(results);

          var data = JSON.parse(jsonParsedString.Rows);
          let headers = ["Transaction_Date","Particulars","Amount",'Cr_Dr', "Balance", "Transaction_Type","Normalized_Party_Name_Label", "Normalized_Charge_Name_Label", "Charge_Class"]

          const seperator = ",";

          const csv = [headers.join(seperator),
                        ...data.map(row => headers.map(field => `${row[field]}`).join(seperator))
                        ]

          let csvContent = "data:text/csv;charset=utf-8," + csv.join("\n");
          var encodedUri = encodeURI(csvContent);
          window.open(encodedUri);

         await test();
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