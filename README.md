#StockappFrontend
This project is using vue. It exchanging data with c# written backend service which exchanging data with 1c(erp system) .
##Problem:
We have DCT(data collection terminal) and it's supposed to make life easier, but using it as it is not quite easy.
Usuall algorithm is:
1) Finding an app in which you have to load all goods data which could be too much.
2) Creating a document in this app.
3) Scanning goods.
4) After scanning is done all data has to be saved in special format which 1c could understand.
5) Open this data and save it into a document.

##Solution:
Using additional apps can help us avoid complicated steps and we'll have only 2 steps:
1) Choose document in which context you'll work.
2) Scan
So you can use DCT just as a barcode scanner with instant confirmation if data is succefully added to 1c. 
