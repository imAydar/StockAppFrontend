<template>
   <div>
      <h3 >{{docNumber}} {{docDate}}</h3>
      <Loader />
      <div id="result">
         <div id="currentBarcode"></div>
         <div id="currentWareName"></div>
      </div>
      <div class="table-responsive">
         <table class="table table-striped table-sm">
            <colgroup>
               <col span="1" style="width: 80%;">
               <col span="1" style="width: 10%;">
               <col span="1" style="width: 10%;">
            </colgroup>
            <tr v-for="(item, index) in rows" v-bind:key="index">
               <td>{{item.WareName}}</td>
               <td>{{item.Quantity}}</td>
               <td>{{item.HasToBeQuantity}}</td>
               <td style="display:none">{{item.Code}}</td>
            </tr>
         </table>
      </div>
   </div>
</template>
<script>
   import Loader from './Loader.vue'
   export default {
       components: {
         Loader
       },
       data() {
           return {
               rows: {},
               docNumber: null,
               docDate: null,
               currentBarcode: null,
               currentWareName: null,
               backendUrl: 'http://192.168.200.110:888/api/DocumentRow'
           };
       },
       mounted() {
           this.getRows();
           this.barcodeHook();
       },
       methods: {
            getRows() {//get all rows in document
               this.load(true);
               let vm = this;
               let url = new URL(location.href);
               this.docNumber = url.searchParams.get("Number");
               this.docDate = url.searchParams.get("Date");
               url = vm.backendUrl + "?" + url.searchParams.toString();
               fetch(url).then(r => {
                   return r.json();
               }).then(data => {
                   this.rows = data;
               }).finally(() => {
                   vm.load(false);
               });
           },
           addRow(wareCode) {
               document.getElementById("result").style.color = 'white';
               document.getElementById("result").style.background = '#41a37a';
               let tds = document.querySelectorAll("td");
               tds.forEach((item) => {
                   if ('"' + item.innerHTML.toString() + '"' == wareCode.toString()) {
                       let parent = item.parentNode;
                       parent.children[1].innerHTML = (Number)(parent.children[1].innerHTML) + 1;
                       document.getElementById('currentWareName').innerHTML = parent.children[0].innerHTML + " +1шт";
                   }
               });
           },
           load(stop) { //in case there'd be more code
               if (stop)
                   document.getElementById('loader').style.display = "";
               else
                   document.getElementById('loader').style.display = "none";
           },
           sendBarcode(data, vm) {
               vm.load(true);
               let url = vm.backendUrl;
               fetch(url, {
                   headers: {'Content-Type': 'application/json'},
                   method: 'Post',
                   body: JSON.stringify(data)
               }).then(response => {
                   return response.text();
               }).then(data => { //sending query to server
                   if (data == '"Ошибка: не найдена номенклатура"') {//haven't found in db
                       document.getElementById("result").style.background = '#a90b0b';
                       document.getElementById("result").style.color = 'white';
                       document.getElementById('currentWareName').innerHTML = "Ошибка: не найдена номенклатура";
                   } else {
                       vm.addRow(data);
                   }
               }).finally(() => {
                   vm.load(false);
               });
           },
           barcodeHook() {
               let vm = this;
               let input = '';
               document.onkeyup = function(event) { //listening to every key up event in our input
                   document.getElementById("result").style.color = 'blue';
                   if (event.keyCode == 13) { //if there was a new line
                       let barcode = input; //check for 13 symbols
                       input = '';
                       let data = {
                           "barcode": barcode,
                           document: {
                               "Number": vm.docNumber,
                               "Date": vm.docDate
                           }
                       }
                       vm.sendBarcode(data, vm);
                   } else {
                       input += event.key;
                   }
               }
           }
       }
   }
</script>
<style>
   /*style for popup*/
   /*#result {position: fixed; bottom: 0; right: 0; z-index:500;}*/
   #result{ position: sticky;top: 0;background: #41a37a;} 
</style>