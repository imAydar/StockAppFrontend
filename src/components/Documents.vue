<template>
   <div class="hello">
      <Loader ref = 'loader'/>
      <div class="table-responsive">
         <table class="table table-striped table-sm">
            <!-- <colgroup>
               <col span="1" style="width: 40%;">
               <col span="1" style="width: 0%;">
               <col span="1" style="width: 30%;">
               <col span="1" style="width: 30%;">
               </colgroup>-->
            <!--<th v-for="(test, name1) in docList[0]" v-bind:key="name1">{{name1}}</th>-->
            <!-- <th>Reference</th>-->
            <!-- <th>userId</th>
               <th>id</th>
               <th>title</th>
               <th>body</th>-->
            <tr v-for="(item, index) in docList" v-bind:key="index">
               <td v-for="(sh, name, ind) in docList[0]" v-bind:key="ind"><a :href='"about?Number=" + item["Number"] + "&Date=" + item["Date"]'>{{docListView[index][name]}}</a></td>
               <!--<td><a :href='"http://192.168.200.110/StockApp/api/DocumentRow?Number=" + item["Number"] + "&Date=" + item["Date"]'>Reference</a></td>-->
               <!--<td><a :href='"about?Number=" + item["Number"] + "&Date=" + item["Date"]'>Reference</a></td>-->
            </tr>
         </table>
      </div>
   </div>
</template>
<script>
   import 'bootstrap/dist/css/bootstrap.css'
   import 'bootstrap-vue/dist/bootstrap-vue.css'
   import Loader from './Loader.vue'
   export default {
       name: 'Documents',
       components: {
         Loader
       },
       data() {
           return {
              backendURl: 'http://192.168.200.110:888/api/document/',
               docList: {},
               docListView: {}
           };
       },
       mounted() {
         this.getdocuments();
       },
       methods: {
           getdocuments() {//get list of last documents 
             this.load(true);
               fetch(this.backendURl).then(r => {
                   return r.json();
               }).then(data => {
                  this.docList = JSON.parse(JSON.stringify(data));
                  data.map(x => {  
                     x.Comment = x.Comment.substr(0, 20); 
                     x.Date = x.Date.replace("T", " ");
                     });  
                  this.docListView = data;
                  
                //  this.docListView = data.map(x => {  x.Comment = x.Comment.substr(0, 20)});       
               }).finally(() =>{
                 this.load(false);
               })
           },
           load(stop){//in case there'd be more code
               if(stop)
                   document.getElementById('loader').style.display = "";
               else
                   document.getElementById('loader').style.display = "none";
           },
   
       }
   }
</script>
<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
   h3 {
   margin: 40px 0 0;
   }
   ul {
   list-style-type: none;
   padding: 0;
   }
   li {
   display: inline-block;
   margin: 0 10px;
   }
   a {
   color: #42b983;
   }
   tr{
   color: #42b983;
   }
   .table-responsive{
   background-color: #3a4254;
   }
   table{
   width: 100%;
   table-layout: fixed;
   word-wrap: break-word;
   }
   table td{
   /*border-bottom: 1px solid #dee2e6;*/
   border-right: 1px solid #4e4f50;
   /*text-align:left;
       LINE-BREAK: unset;*/
   }
   .table-sm td{
   padding:0;
   font-size: 15.4px;
   }
</style>