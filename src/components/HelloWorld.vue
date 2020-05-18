<template>
  <div class="hello">
  <div class="lds-roller" id="loader" style="display:none"><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div></div>
     <table class="greyGridTable">
 
    <thead>
      <tr>
      <th v-for="(test, name1) in docList[0]" v-bind:key="name1">{{name1}}</th>
     <!-- <th>Reference</th>-->
       <!-- <th>userId</th>
        <th>id</th>
        <th>title</th>
        <th>body</th>-->
      </tr>
    </thead>
    <tr v-for="(item, index) in docList" v-bind:key="index">
        <td v-for="(sh, name, ind) in docList[0]" v-bind:key="ind"><a :href='"about?Number=" + item["Number"] + "&Date=" + item["Date"]'>{{item[name]}}</a></td>
    <!--<td><a :href='"http://192.168.200.110/StockApp/api/DocumentRow?Number=" + item["Number"] + "&Date=" + item["Date"]'>Reference</a></td>-->
    <!--<td><a :href='"about?Number=" + item["Number"] + "&Date=" + item["Date"]'>Reference</a></td>-->
        </tr>
  </table>
  </div>
</template>

<script>
//import axios from 'axios';
export default {
    name: 'Documents',
    data() {
        return {
            docList: {},
            msg: null
        };
    },
    mounted() {
      this.getdocuments();
    },
    methods: {
        getdocuments() {
          this.load(true);
            var url = 'http://192.168.200.110/StockApp/api/document/';
            this.msg = url;
            fetch(url).then(r => {
                this.msg = "ok";
                return r.json();
            }).then(data => {

                this.docList = data;
                this.msg = "ok";
                console.log(data);
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
table.greyGridTable {
  border: 2px solid #FFFFFF;
  width: 100%;
  text-align: center;
  border-collapse: collapse;
  table-layout:fixed;
}
table.greyGridTable td, table.greyGridTable th {
  border: 1px solid #FFFFFF;
  padding: 5px 10px;

}
table.greyGridTable tbody td {
  font-size: 13px;
   text-align: left;
}
table.greyGridTable tr:nth-child(even) {
  background: #D0E4F5;
}
table.greyGridTable thead {
  background: #FFFFFF;
  border-bottom: 4px solid #333333;
}
table.greyGridTable thead th {
  font-size: 15px;
  font-weight: bold;
  color: #333333;
  text-align: center;
  border-left: 2px solid #333333;
}
table.greyGridTable thead th:first-child {
  border-left: none;
}

table.greyGridTable tfoot {
  font-size: 14px;
  font-weight: bold;
  color: #333333;
  border-top: 4px solid #333333;
}
table.greyGridTable tfoot td {
  font-size: 14px;
}


.lds-roller {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
}
.lds-roller div {
  animation: lds-roller 1.2s cubic-bezier(0.5, 0, 0.5, 1) infinite;
  transform-origin: 40px 40px;
}
.lds-roller div:after {
  content: " ";
  display: block;
  position: absolute;
  width: 7px;
  height: 7px;
  border-radius: 50%;
  background: #58c0f5;
  margin: -4px 0 0 -4px;
}
.lds-roller div:nth-child(1) {
  animation-delay: -0.036s;
}
.lds-roller div:nth-child(1):after {
  top: 63px;
  left: 63px;
}
.lds-roller div:nth-child(2) {
  animation-delay: -0.072s;
}
.lds-roller div:nth-child(2):after {
  top: 68px;
  left: 56px;
}
.lds-roller div:nth-child(3) {
  animation-delay: -0.108s;
}
.lds-roller div:nth-child(3):after {
  top: 71px;
  left: 48px;
}
.lds-roller div:nth-child(4) {
  animation-delay: -0.144s;
}
.lds-roller div:nth-child(4):after {
  top: 72px;
  left: 40px;
}
.lds-roller div:nth-child(5) {
  animation-delay: -0.18s;
}
.lds-roller div:nth-child(5):after {
  top: 71px;
  left: 32px;
}
.lds-roller div:nth-child(6) {
  animation-delay: -0.216s;
}
.lds-roller div:nth-child(6):after {
  top: 68px;
  left: 24px;
}
.lds-roller div:nth-child(7) {
  animation-delay: -0.252s;
}
.lds-roller div:nth-child(7):after {
  top: 63px;
  left: 17px;
}
.lds-roller div:nth-child(8) {
  animation-delay: -0.288s;
}
.lds-roller div:nth-child(8):after {
  top: 56px;
  left: 12px;
}
@keyframes lds-roller {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}


</style>
