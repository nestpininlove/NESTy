<!DOCTYPE html>
<html lang="th">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>ฐานข้อมูลนักเรียน แผนกการจัดการสำนักงาน</title>

<style>
body{
  font-family: "Sarabun", Arial, sans-serif;
  background:#f2f6ff;
  margin:0;
}
header{
  background:#1e3a8a;
  color:white;
  padding:20px;
  text-align:center;
}
input{
  padding:8px;
  width:320px;
  margin:15px auto;
  display:block;
  border-radius:5px;
  border:1px solid #ccc;
}
table{
  width:95%;
  margin:0 auto 30px;
  border-collapse:collapse;
  background:white;
}
th,td{
  border:1px solid #ddd;
  padding:10px;
  text-align:center;
  font-size:14px;
}
th{
  background:#2563eb;
  color:white;
}
tr:nth-child(even){
  background:#f0f4ff;
}
</style>
</head>

<body>

<header>
  <h2>ฐานข้อมูลรายชื่อนักเรียน</h2>
  <p>แผนกการจัดการสำนักงาน</p>
</header>

<input type="text" id="search" placeholder="ค้นหาข้อมูลนักเรียน..." onkeyup="searchTable()">

<table id="studentTable">
  <thead>
    <tr>
      <th>เลขที่</th>
      <th>ชื่อ-สกุล</th>
      <th>ชั้น/ห้อง</th>
      <th>เบอร์โทรนักศึกษา</th>
      <th>อีเมล</th>
      <th>ที่อยู่</th>
      <th>เบอร์ผู้ปกครอง</th>
    </tr>
  </thead>
  <tbody id="data">
    <!-- ข้อมูลจะถูกดึงมาจาก Google Sheets -->
  </tbody>
</table>

<script>
// 🔹 1xEWhm1oqs1Tyqnuo05RgOZfu-eT45VdGBvHrySExlKE/edit?usp=sharing
const SHEET_ID = "1xEWhm1oqs1Tyqnuo05RgOZfu-eT45VdGBvHrySExlKE/edit?usp=sharing";
const SHEET_NAME = "ชีต1";

// URL สำหรับดึงข้อมูล
const url = `https://docs.google.com/spreadsheets/d/${SHEET_ID}/gviz/tq?tqx=out:json&sheet=${SHEET_NAME}`;

fetch(url)
.then(res => res.text())
.then(text => {
  const json = JSON.parse(text.substring(47).slice(0,-2));
  const rows = json.table.rows;

  let html = "";
  rows.forEach(r => {
    html += `
      <tr>
        <td>${r.c[0]?.v ?? ""}</td>
        <td>${r.c[1]?.v ?? ""}</td>
        <td>${r.c[2]?.v ?? ""}</td>
        <td>${r.c[3]?.v ?? ""}</td>
        <td>${r.c[4]?.v ?? ""}</td>
        <td>${r.c[5]?.v ?? ""}</td>
        <td>${r.c[6]?.v ?? ""}</td>
      </tr>
    `;
  });

  document.getElementById("data").innerHTML = html;
});

// 🔍 ฟังก์ชันค้นหา
function searchTable(){
  let input = document.getElementById("search").value.toLowerCase();
  let rows = document.querySelectorAll("#studentTable tbody tr");

  rows.forEach(row=>{
    let text = row.innerText.toLowerCase();
    row.style.display = text.includes(input) ? "" : "none";
  });
}
</script>

</body>
</html>
