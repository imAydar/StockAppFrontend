<template>
    <div>
    <h3 >{{docNumber}} {{docDate}}</h3>
    <div class="lds-roller" id="loader" style="display:none"><div></div><div></div><div></div><div></div><div></div><div></div><div></div><div></div></div>
    <div id="result"><div id="currentBarcode">{{currentBarcode}}</div><div id="currentWareName">{{currentWareName}}</div></div>
        <table class="greyGridTable" style="width: 100%">
            <colgroup>
       <col span="1" style="width: 80%;">
       <col span="1" style="width: 10%;">
       <col span="1" style="width: 10%;">
    </colgroup>
     <!-- <th v-for="(test, name1) in rows[0]" v-bind:key="name1">{{name1}}</th>-->
        <!--<th>Товар</th>
        <th>Количество</th>
        <th>Должно быть</th>
      </tr>
    </thead>-->
    <tr v-for="(item, index) in rows" v-bind:key="index">
    <td>{{item.WareName}}</td>
    <td>{{item.Quantity}}</td>
    <td>{{item.HasToBeQuantity}}</td>
    <td style="display:none">{{item.Code}}</td>
    <!--<td v-for="(sh, name, ind) in rows[0]" v-bind:key="ind">{{item[name]}}</td>-->
        </tr>
  </table>
    </div>
</template>
<script>
export default {
    data() {
        return {
            rows: {},
            docNumber: null,
            docDate: null,
            currentBarcode: null,
            currentWareName: null
        };
    },
    mounted() {
        this.getRows();
        this.barcodeHook();
    },
    methods: {
        getRows() {
            this.load(true);
            var vm = this;
            var url = new URL(location.href);
            this.docNumber = url.searchParams.get("Number");
            this.docDate = url.searchParams.get("Date");
            var params = url.searchParams.toString();
            url = "http://192.168.200.110/StockApp/api/DocumentRow?" + params;
            fetch(url).then(r => {
                return r.json();
            }).then(data => {
                this.rows = data;
                console.log(data);
            }).finally(() => {
                vm.load(false);
            });
        },
        addRow(wareCode) {
            document.getElementById("result").style.color = 'green';
            var tds = document.querySelectorAll("td");
            tds.forEach((userItem) => {
                if ('"' + userItem.innerHTML.toString() + '"' == wareCode.toString()) {
                    var parent = userItem.parentNode;
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
            console.log(document.querySelector('.lds-roller').style.display);
            var url = 'http://192.168.200.110/StockApp/api/DocumentRow/';
            fetch(url, {
                headers: {
                    'Content-Type': 'application/json',
                    'Accept-Charset': 'utf8'
                },
                method: 'Post',
                body: JSON.stringify(data)
            }).then(response => {
                return response.text();
            }).then(data => { //sending query to server
                console.log(data);
                if (data == '"Ошибка: не найдена номенклатура"') {
                    document.getElementById('currentWareName').innerHTML = data;
                    document.getElementById("result").style.color = 'red';
                } else {
                    vm.addRow(data);
                }
            }).finally(() => {
                vm.load(false);
            });
        },
        barcodeHook() {
            var vm = this;
            var input = '';
            document.onkeyup = function(event) { //listening to every key up event in our input
                document.getElementById("result").style.color = 'blue';
                document.getElementById('currentWareName').innerHTML = '';
                if (event.keyCode == 13) { //if there was a new line

                    var barcode = input; //check for 13 symbols
                    document.getElementById('currentBarcode').innerHTML = barcode;
                    var data = {
                        "barcode": barcode,
                        document: {
                            "Number": vm.docNumber,
                            "Date": vm.docDate
                        }
                    }
                    vm.sendBarcode(data, vm);
                    input = '';
                } else {
                    input += event.key;
                }
            }
        }
    }
}
</script>
<style>

</style>