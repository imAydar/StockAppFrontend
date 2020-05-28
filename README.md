# StockappFrontend
This project is using vue. It exchanging data with c# written backend service which exchanging data with 1c(erp system) . 

![gif](/public/gitAssests/main.gif)

## Problem:
We have DCT(data collection terminal) and it's supposed to make life easier, but using it as it is not quite easy.</br>
Usuall algorithm is:
* Finding an app in which you have to load all goods data which could be too much.
* Creating a document in this app.
* Scanning goods.
* After scanning is done all data has to be saved in special format which 1c could understand.
* Open this data and save it into a document.

## Solution:
Using additional apps can help us avoid complicated steps and we'll have only 2 steps:
* Choose document in which context you'll work.
* Scan </br>

So you can use DCT just as a barcode scanner with instant confirmation if data is succefully added to 1c. 
