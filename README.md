<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>نظام الكاشير السعودي</title>

<style>
*{box-sizing:border-box}
body{
    margin:0;
    font-family:Tahoma,Arial,sans-serif;
    background:#f4f6f8;
    color:#222
}
button,input,select{
    font-family:inherit
}
button{
    cursor:pointer;
    border:0;
    border-radius:8px;
    padding:11px 16px;
    background:#111827;
    color:white
}
button:hover{opacity:.9}
.container{
    max-width:1250px;
    margin:auto;
    padding:20px
}
.card{
    background:white;
    border-radius:14px;
    padding:20px;
    margin-bottom:20px;
    box-shadow:0 3px 15px #00000010
}
h1,h2,h3{margin-top:0}
input,select{
    width:100%;
    padding:12px;
    border:1px solid #ddd;
    border-radius:8px;
    margin-top:6px;
    margin-bottom:12px
}
label{
    font-weight:bold;
    display:block
}
.grid{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(220px,1fr));
    gap:15px
}
.stats{
    display:grid;
    grid-template-columns:repeat(auto-fit,minmax(180px,1fr));
    gap:15px
}
.stat{
    background:white;
    padding:20px;
    border-radius:14px;
    box-shadow:0 3px 15px #00000010
}
.stat strong{
    display:block;
    font-size:25px;
    margin-top:8px
}
.nav{
    display:flex;
    flex-wrap:wrap;
    gap:8px;
    margin-bottom:20px
}
.nav button{
    background:#374151
}
.hidden{display:none!important}
table{
    width:100%;
    border-collapse:collapse
}
th,td{
    padding:10px;
    border-bottom:1px solid #eee;
    text-align:right
}
th{background:#f3f4f6}
.product-grid{
    display:grid;
    grid-template-columns:repeat(auto-fill,minmax(170px,1fr));
    gap:12px
}
.product{
    background:white;
    border:1px solid #eee;
    border-radius:12px;
    padding:15px;
    cursor:pointer
}
.product:hover{
    border-color:#111827
}
.total{
    font-size:22px;
    font-weight:bold
}
.danger{background:#dc2626}
.success{background:#16a34a}
.blue{background:#2563eb}
.orange{background:#ea580c}

#setup{
    min-height:100vh;
    display:flex;
    align-items:center;
    justify-content:center;
    padding:20px
}
.setup-box{
    max-width:750px;
    width:100%;
    background:white;
    padding:30px;
    border-radius:18px;
    box-shadow:0 10px 40px #0002
}

.invoice{
    background:white;
    max-width:800px;
    margin:auto;
    padding:30px
}
.invoice-header{
    display:flex;
    justify-content:space-between;
    gap:20px;
    border-bottom:2px solid #111;
    padding-bottom:15px
}
.qr{
    text-align:center;
    margin:20px
}
.footer{
    text-align:center;
    color:#777;
    margin-top:30px
}

@media print{
    body{background:white}
    body *{visibility:hidden}
    #printArea,#printArea *{visibility:visible}
    #printArea{
        position:absolute;
        left:0;
        top:0;
        width:100%
    }
}
</style>
</head>

<body>

<!-- إعداد أول تشغيل -->
<section id="setup">

<div class="setup-box">

<h1>⚙️ إعداد نظام الكاشير</h1>

<p>
أدخل بيانات نشاطك التجاري. هذه البيانات ستظهر على فواتيرك.
</p>

<div class="grid">

<div>
<label>اسم المنشأة / الاسم القانوني</label>
<input id="businessName">
</div>

<div>
<label>اسم النشاط</label>
<input id="activityName">
</div>

<div>
<label>الرقم الضريبي VAT</label>
<input id="vatNumber">
</div>

<div>
<label>السجل التجاري</label>
<input id="crNumber">
</div>

<div>
<label>رقم الهاتف</label>
<input id="phone">
</div>

<div>
<label>المدينة</label>
<input id="city">
</div>

<div>
<label>العنوان</label>
<input id="address">
</div>

<div>
<label>الفرع</label>
<input id="branch" value="الفرع الرئيسي">
</div>

<div>
<label>نسبة الضريبة %</label>
<input id="vatRate" type="number" value="15">
</div>

<div>
<label>رقم بداية الفاتورة</label>
<input id="invoiceStart" type="number" value="1">
</div>

</div>

<button onclick="saveSetup()" class="success">
حفظ وبدء النظام
</button>

</div>
</section>


<!-- التطبيق -->
<div id="app" class="hidden">

<div class="container">

<h1>🧾 نظام الكاشير</h1>

<div id="businessHeader"></div>

<div class="nav">

<button onclick="showPage('dashboard')">الرئيسية</button>
<button onclick="showPage('pos')">الكاشير</button>
<button onclick="showPage('products')">المنتجات</button>
<button onclick="showPage('invoices')">الفواتير</button>
<button onclick="showPage('customers')">العملاء</button>
<button onclick="showPage('reports')">التقارير</button>
<button onclick="showPage('settings')">الإعدادات</button>

</div>


<!-- الرئيسية -->

<section id="dashboard" class="page">

<div class="stats">

<div class="stat">
المبيعات اليوم
<strong id="todaySales">0.00 ر.س</strong>
</div>

<div class="stat">
عدد الفواتير
<strong id="invoiceCount">0</strong>
</div>

<div class="stat">
المنتجات
<strong id="productCount">0</strong>
</div>

<div class="stat">
ضريبة المبيعات
<strong id="todayVat">0.00 ر.س</strong>
</div>

</div>

</section>


<!-- الكاشير -->

<section id="pos" class="page hidden">

<div class="card">

<h2>🛒 الكاشير</h2>

<div class="grid">

<div>
<label>نوع الفاتورة</label>

<select id="invoiceType">

<option value="B2C">فاتورة ضريبية مبسطة B2C</option>
<option value="B2B">فاتورة ضريبية B2B</option>

</select>
</div>

<div>
<label>طريقة الدفع</label>

<select id="paymentMethod">

<option>نقدي</option>
<option>شبكة</option>
<option>بطاقة</option>
<option>تحويل بنكي</option>
<option>آجل</option>

</select>
</div>

<div>
<label>رقم ضريبة العميل B2B</label>
<input id="customerVat">
</div>

</div>

<input
id="productSearch"
placeholder="🔎 ابحث عن منتج..."
oninput="renderProducts()">

<div id="productGrid" class="product-grid"></div>

</div>


<div class="card">

<h2>الفاتورة الحالية</h2>

<table>

<thead>
<tr>
<th>المنتج</th>
<th>الكمية</th>
<th>السعر</th>
<th>الإجمالي</th>
<th></th>
</tr>
</thead>

<tbody id="cart"></tbody>

</table>

<br>

<div class="total">
الإجمالي قبل الضريبة:
<span id="subtotal">0.00</span> ر.س
</div>

<div class="total">
الضريبة:
<span id="vatTotal">0.00</span> ر.س
</div>

<div class="total">
الإجمالي:
<span id="grandTotal">0.00</span> ر.س
</div>

<br>

<button class="success" onclick="completeSale()">
💰 إصدار الفاتورة
</button>

<button class="danger" onclick="clearCart()">
مسح
</button>

</div>

</section>


<!-- المنتجات -->

<section id="products" class="page hidden">

<div class="card">

<h2>📦 المنتجات</h2>

<div class="grid">

<input id="newProductName" placeholder="اسم المنتج">

<input id="newProductBarcode" placeholder="الباركود">

<input id="newProductPrice" type="number" placeholder="السعر">

<input id="newProductQty" type="number" placeholder="المخزون">

</div>

<button onclick="addProduct()" class="success">
إضافة منتج
</button>

</div>

<div class="card">

<table>

<thead>
<tr>
<th>المنتج</th>
<th>الباركود</th>
<th>السعر</th>
<th>المخزون</th>
<th>حذف</th>
</tr>
</thead>

<tbody id="productsTable"></tbody>

</table>

</div>

</section>


<!-- الفواتير -->

<section id="invoices" class="page hidden">

<div class="card">

<h2>🧾 الفواتير</h2>

<table>

<thead>
<tr>
<th>رقم</th>
<th>التاريخ</th>
<th>النوع</th>
<th>الإجمالي</th>
<th>عرض</th>
</tr>
</thead>

<tbody id="invoicesTable"></tbody>

</table>

</div>

</section>


<!-- العملاء -->

<section id="customers" class="page hidden">

<div class="card">

<h2>👥 العملاء</h2>

<div class="grid">

<input id="customerName" placeholder="اسم العميل">

<input id="customerPhone" placeholder="رقم الهاتف">

<input id="customerVAT" placeholder="الرقم الضريبي">

</div>

<button onclick="addCustomer()" class="success">
إضافة عميل
</button>

</div>

<div class="card">

<table>

<thead>
<tr>
<th>الاسم</th>
<th>الهاتف</th>
<th>الرقم الضريبي</th>
</tr>
</thead>

<tbody id="customersTable"></tbody>

</table>

</div>

</section>


<!-- التقارير -->

<section id="reports" class="page hidden">

<div class="card">

<h2>📊 التقارير</h2>

<p>
إجمالي المبيعات:
<strong id="reportSales">0</strong> ر.س
</p>

<p>
إجمالي الضريبة:
<strong id="reportVat">0</strong> ر.س
</p>

<p>
عدد الفواتير:
<strong id="reportInvoices">0</strong>
</p>

</div>

</section>


<!-- الإعدادات -->

<section id="settings" class="page hidden">

<div class="card">

<h2>⚙️ بيانات النشاط</h2>

<div class="grid">

<input id="setBusinessName" placeholder="اسم المنشأة">

<input id="setActivityName" placeholder="النشاط">

<input id="setVAT" placeholder="الرقم الضريبي">

<input id="setCR" placeholder="السجل التجاري">

<input id="setPhone" placeholder="الهاتف">

<input id="setCity" placeholder="المدينة">

<input id="setAddress" placeholder="العنوان">

<input id="setBranch" placeholder="الفرع">

</div>

<button onclick="updateSettings()" class="success">
حفظ البيانات
</button>

<button onclick="resetSystem()" class="danger">
مسح بيانات النظام
</button>

</div>

</section>

</div>

</div>


<!-- منطقة الطباعة -->

<div id="printArea"></div>


<script>

/* =========================
   قاعدة البيانات المحلية
========================= */

let db = JSON.parse(localStorage.getItem("SAUDI_POS_DB")) || {

setup:false,

business:{},

products:[],

customers:[],

invoices:[],

journal:[],

nextInvoice:1

};

let cart=[];


/* =========================
   حفظ قاعدة البيانات
========================= */

function saveDB(){

localStorage.setItem(
"SAUDI_POS_DB",
JSON.stringify(db)
);

}


/* =========================
   بدء النظام
========================= */

function init(){

if(!db.setup){

document.getElementById("setup")
.classList.remove("hidden");

document.getElementById("app")
.classList.add("hidden");

}else{

document.getElementById("setup")
.classList.add("hidden");

document.getElementById("app")
.classList.remove("hidden");

loadBusiness();

refreshAll();

}

}


/* =========================
   حفظ الإعداد
========================= */

function saveSetup(){

db.business={

name:businessName.value.trim(),

activity:activityName.value.trim(),

vat:vatNumber.value.trim(),

cr:crNumber.value.trim(),

phone:phone.value.trim(),

city:city.value.trim(),

address:address.value.trim(),

branch:branch.value.trim(),

vatRate:Number(vatRate.value)||15

};

db.nextInvoice=
Number(invoiceStart.value)||1;

db.setup=true;

saveDB();

init();

}


/* =========================
   بيانات النشاط
========================= */

function loadBusiness(){

let b=db.business;

businessHeader.innerHTML=`

<div class="card">

<strong>${esc(b.name)}</strong><br>

${esc(b.activity)}<br>

الرقم الضريبي:
${esc(b.vat)}<br>

السجل التجاري:
${esc(b.cr)}<br>

${esc(b.city)} - ${esc(b.address)}

</div>

`;

setBusinessName.value=b.name||"";
setActivityName.value=b.activity||"";
setVAT.value=b.vat||"";
setCR.value=b.cr||"";
setPhone.value=b.phone||"";
setCity.value=b.city||"";
setAddress.value=b.address||"";
setBranch.value=b.branch||"";

}


/* =========================
   التنقل
========================= */

function showPage(page){

document
.querySelectorAll(".page")
.forEach(x=>x.classList.add("hidden"));

document
.getElementById(page)
.classList.remove("hidden");

refreshAll();

}


/* =========================
   المنتجات
========================= */

function addProduct(){

let name=newProductName.value.trim();

let barcode=newProductBarcode.value.trim();

let price=Number(newProductPrice.value);

let qty=Number(newProductQty.value);

if(!name || price<=0){

alert("أدخل اسم المنتج والسعر");

return;

}

db.products.push({

id:Date.now(),

name,

barcode,

price,

qty

});

saveDB();

newProductName.value="";
newProductBarcode.value="";
newProductPrice.value="";
newProductQty.value="";

renderProducts();

}


function renderProducts(){

let search=
(productSearch.value||"").toLowerCase();

productGrid.innerHTML="";

db.products

.filter(p=>
p.name.toLowerCase().includes(search) ||
p.barcode.includes(search)
)

.forEach(p=>{

let div=document.createElement("div");

div.className="product";

div.innerHTML=`

<strong>${esc(p.name)}</strong>

<br>

${p.price.toFixed(2)} ر.س

<br>

المخزون: ${p.qty}

`;

div.onclick=()=>addToCart(p.id);

productGrid.appendChild(div);

});

}


function renderProductsTable(){

productsTable.innerHTML="";

db.products.forEach(p=>{

productsTable.innerHTML+=`

<tr>

<td>${esc(p.name)}</td>

<td>${esc(p.barcode)}</td>

<td>${p.price.toFixed(2)}</td>

<td>${p.qty}</td>

<td>

<button
class="danger"
onclick="deleteProduct(${p.id})">

حذف

</button>

</td>

</tr>

`;

});

}


function deleteProduct(id){

if(!confirm("حذف المنتج؟"))return;

db.products=
db.products.filter(p=>p.id!==id);

saveDB();

refreshAll();

}


/* =========================
   السلة
========================= */

function addToCart(id){

let p=db.products.find(x=>x.id===id);

if(!p)return;

let item=cart.find(x=>x.id===id);

if(item){

if(item.qty>=p.qty){

alert("المخزون غير كافٍ");

return;

}

item.qty++;

}else{

cart.push({

id:p.id,

name:p.name,

price:p.price,

qty:1

});

}

renderCart();

}


function renderCart(){

cart.innerHTML="";

let subtotal=0;

window.cart.forEach(item=>{

let total=item.price*item.qty;

subtotal+=total;

cart.innerHTML+=`

<tr>

<td>${esc(item.name)}</td>

<td>

<button onclick="changeQty(${item.id},-1)">-</button>

${item.qty}

<button onclick="changeQty(${item.id},1)">+</button>

</td>

<td>${item.price.toFixed(2)}</td>

<td>${total.toFixed(2)}</td>

<td>

<button
class="danger"
onclick="removeCart(${item.id})">

X

</button>

</td>

</tr>

`;

});

let rate=(db.business.vatRate||15)/100;

let vat=subtotal*rate;

let total=subtotal+vat;

document.getElementById("subtotal").textContent=
subtotal.toFixed(2);

document.getElementById("vatTotal").textContent=
vat.toFixed(2);

document.getElementById("grandTotal").textContent=
total.toFixed(2);

}


function changeQty(id,amount){

let item=cart.find(x=>x.id===id);

let p=db.products.find(x=>x.id===id);

if(!item)return;

item.qty+=amount;

if(item.qty>p.qty)
item.qty=p.qty;

if(item.qty<=0)
removeCart(id);

renderCart();

}


function removeCart(id){

cart=cart.filter(x=>x.id!==id);

renderCart();

}


function clearCart(){

cart=[];

renderCart();

}


/* =========================
   إصدار الفاتورة
========================= */

function completeSale(){

if(cart.length===0){

alert("الفاتورة فارغة");

return;

}

let type=invoiceType.value;

let customerVatValue=
customerVat.value.trim();

if(type==="B2B" && !customerVatValue){

alert(
"في الفاتورة B2B أدخل الرقم الضريبي للمشتري المسجل في ضريبة القيمة المضافة."
);

return;

}

let subtotal=
cart.reduce(
(sum,x)=>sum+x.price*x.qty,
0
);

let rate=(db.business.vatRate||15)/100;

let vat=subtotal*rate;

let total=subtotal+vat;

let invoiceNumber=
String(db.nextInvoice).padStart(6,"0");

let date=
new Date().toISOString();

let invoice={

number:invoiceNumber,

date,

type,

customerVat:customerVatValue,

payment:paymentMethod.value,

items:JSON.parse(JSON.stringify(cart)),

subtotal,

vat,

total

};

db.invoices.push(invoice);


/* خصم المخزون */

cart.forEach(item=>{

let p=db.products.find(x=>x.id===item.id);

if(p)p.qty-=item.qty;

});


/* القيد المحاسبي */

createJournal(invoice);


db.nextInvoice++;

saveDB();

printInvoice(invoice);

cart=[];

renderCart();

refreshAll();

}


/* =========================
   القيد المزدوج
========================= */

function createJournal(invoice){

let entries=[];

if(invoice.payment==="آجل"){

entries.push({

account:"العملاء",

debit:invoice.total,

credit:0

});

}else{

entries.push({

account:invoice.payment==="نقدي"
?"الصندوق"
:"البنك",

debit:invoice.total,

credit:0

});

}

entries.push({

account:"المبيعات",

debit:0,

credit:invoice.subtotal

});

entries.push({

account:"ضريبة القيمة المضافة - مخرجات",

debit:0,

credit:invoice.vat

});

db.journal.push({

date:invoice.date,

invoice:invoice.number,

entries

});

}


/* =========================
   QR - مرحلة أولى
========================= */

function generateZATCAQR(invoice){

let fields=[

db.business.name,

db.business.vat,

new Date(invoice.date).toISOString(),

invoice.total.toFixed(2),

invoice.vat.toFixed(2)

];

let bytes=[];

fields.forEach((value,index)=>{

let data=new TextEncoder().encode(value);

bytes.push(index+1);

bytes.push(data.length);

data.forEach(b=>bytes.push(b));

});

let binary=
String.fromCharCode(...bytes);

return btoa(binary);

}


/*
ملاحظة:
لإظهار QR بصرياً يحتاج النظام إلى
مكتبة QR أو مولد QR مدمج.
*/


/* =========================
   طباعة الفاتورة
========================= */

function printInvoice(invoice){

let qr=generateZATCAQR(invoice);

let items="";

invoice.items.forEach(item=>{

items+=`

<tr>

<td>${esc(item.name)}</td>

<td>${item.qty}</td>

<td>${item.price.toFixed(2)}</td>

<td>
${(item.qty*item.price).toFixed(2)}
</td>

</tr>

`;

});

printArea.innerHTML=`

<div class="invoice">

<div class="invoice-header">

<div>

<h2>${esc(db.business.name)}</h2>

${esc(db.business.activity)}<br>

الرقم الضريبي:
${esc(db.business.vat)}<br>

السجل التجاري:
${esc(db.business.cr)}<br>

${esc(db.business.address)}

</div>

<div>

<strong>
${invoice.type==="B2C"
?"فاتورة ضريبية مبسطة"
:"فاتورة ضريبية"}
</strong>

<br>

رقم الفاتورة:
${invoice.number}

<br>

التاريخ:
${new Date(invoice.date).toLocaleString("ar-SA")}

</div>

</div>

<br>

<table>

<thead>

<tr>

<th>المنتج</th>
<th>الكمية</th>
<th>السعر</th>
<th>الإجمالي</th>

</tr>

</thead>

<tbody>

${items}

</tbody>

</table>

<hr>

<p>
الإجمالي قبل الضريبة:
<strong>
${invoice.subtotal.toFixed(2)} ر.س
</strong>
</p>

<p>
ضريبة القيمة المضافة:
<strong>
${invoice.vat.toFixed(2)} ر.س
</strong>
</p>

<h2>
الإجمالي:
${invoice.total.toFixed(2)} ر.س
</h2>

${invoice.type==="B2B"
?
`<p>الرقم الضريبي للمشتري:
<strong>${esc(invoice.customerVat)}</strong></p>`
:""}

<div class="qr">

<strong>بيانات QR للفاتورة</strong>

<br><br>

<div style="
word-break:break-all;
font-size:10px;
border:1px solid #ddd;
padding:10px
">

${qr}

</div>

</div>

<div class="footer">

تصميم المهندس صلاح علي

</div>

</div>

`;

window.print();

}


/* =========================
   الفواتير
========================= */

function renderInvoices(){

invoicesTable.innerHTML="";

db.invoices
.slice()
.reverse()
.forEach((inv,index)=>{

invoicesTable.innerHTML+=`

<tr>

<td>${inv.number}</td>

<td>
${new Date(inv.date).toLocaleString("ar-SA")}
</td>

<td>
${inv.type}
</td>

<td>
${inv.total.toFixed(2)} ر.س
</td>

<td>

<button
onclick='reprint(${index})'>

طباعة

</button>

</td>

</tr>

`;

});

}


function reprint(index){

let inv=
db.invoices
.slice()
.reverse()[index];

printInvoice(inv);

}


/* =========================
   العملاء
========================= */

function addCustomer(){

let name=customerName.value.trim();

if(!name)return;

db.customers.push({

name,

phone:customerPhone.value.trim(),

vat:customerVAT.value.trim()

});

saveDB();

customerName.value="";
customerPhone.value="";
customerVAT.value="";

renderCustomers();

}


function renderCustomers(){

customersTable.innerHTML="";

db.customers.forEach(c=>{

customersTable.innerHTML+=`

<tr>

<td>${esc(c.name)}</td>

<td>${esc(c.phone)}</td>

<td>${esc(c.vat)}</td>

</tr>

`;

});

}


/* =========================
   التقارير
========================= */

function renderReports(){

let sales=
db.invoices.reduce(
(s,i)=>s+i.total,0
);

let vat=
db.invoices.reduce(
(s,i)=>s+i.vat,0
);

reportSales.textContent=
sales.toFixed(2);

reportVat.textContent=
vat.toFixed(2);

reportInvoices.textContent=
db.invoices.length;

let today=
new Date().toISOString().slice(0,10);

let todayInvoices=
db.invoices.filter(i=>
i.date.slice(0,10)===today
);

todaySales.textContent=
todayInvoices
.reduce((s,i)=>s+i.total,0)
.toFixed(2)+" ر.س";

todayVat.textContent=
todayInvoices
.reduce((s,i)=>s+i.vat,0)
.toFixed(2)+" ر.س";

invoiceCount.textContent=
db.invoices.length;

productCount.textContent=
db.products.length;

}


/* =========================
   الإعدادات
========================= */

function updateSettings(){

db.business.name=setBusinessName.value;

db.business.activity=setActivityName.value;

db.business.vat=setVAT.value;

db.business.cr=setCR.value;

db.business.phone=setPhone.value;

db.business.city=setCity.value;

db.business.address=setAddress.value;

db.business.branch=setBranch.value;

saveDB();

loadBusiness();

alert("تم حفظ البيانات");

}


function resetSystem(){

if(!confirm(
"تحذير: سيتم حذف جميع البيانات!"
))return;

localStorage.removeItem("SAUDI_POS_DB");

location.reload();

}


/* =========================
   تحديث الواجهة
========================= */

function refreshAll(){

loadBusiness();

renderProducts();

renderProductsTable();

renderCart();

renderInvoices();

renderCustomers();

renderReports();

}


/* =========================
   حماية HTML
========================= */

function esc(value){

return String(value??"")
.replace(/&/g,"&amp;")
.replace(/</g,"&lt;")
.replace(/>/g,"&gt;")
.replace(/"/g,"&quot;")
.replace(/'/g,"&#039;");

}


/* تشغيل */

init();

</script>

</body>
</html>
