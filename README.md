<!DOCTYPE html>
<html lang="zh-Hant">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<title>包裹平均秒數計算器</title>
<style>
  body { font-family: Arial, sans-serif; max-width: 420px; margin: 20px auto; padding: 10px; }
  h2 { text-align: center; }
  fieldset { border: 1px solid #ccc; border-radius: 6px; margin-bottom: 20px; padding: 10px 15px; }
  legend { font-weight: bold; padding: 0 10px; }
  label { display: block; margin-top: 10px; font-weight: 600; }
  .time-inputs { display: flex; gap: 8px; margin-top: 6px; }
  .time-inputs input[type="number"] { flex: 1; padding: 6px; font-size: 1em; }
  input[type="number"] { width: 100%; padding: 8px; font-size: 1em; box-sizing: border-box; }
  button { width: 100%; padding: 12px; margin-top: 12px; font-size: 1em; cursor: pointer; border-radius: 4px; border: 1px solid #007bff; background-color: #007bff; color: white; }
  button:hover { background-color: #0056b3; border-color: #0056b3; }
  .result { background: #f8f9fa; border: 1px solid #ddd; border-radius: 6px; padding: 10px; margin-top: 12px; min-height: 30px; }
</style>
</head>
<body>

<h2>包裹平均秒數計算器</h2>

<fieldset>
  <legend>第一次上架</legend>
  <label>開始時間</label>
  <div class="time-inputs">
    <input id="firstStartHour" type="number" min="0" max="23" placeholder="時" />
    <input id="firstStartMinute" type="number" min="0" max="59" placeholder="分" />
    <input id="firstStartSecond" type="number" min="0" max="59" placeholder="秒" />
  </div>

  <label>結束時間</label>
  <div class="time-inputs">
    <input id="firstEndHour" type="number" min="0" max="23" placeholder="時" />
    <input id="firstEndMinute" type="number" min="0" max="59" placeholder="分" />
    <input id="firstEndSecond" type="number" min="0" max="59" placeholder="秒" />
  </div>

  <label>包裹數量</label>
  <input id="firstQuantity" type="number" min="1" placeholder="輸入包裹數量" />

  <button id="calcFirst">計算第一次上架平均秒數</button>
  <div class="result" id="resultFirst">尚未計算</div>
</fieldset>

<fieldset>
  <legend>第二次上架</legend>
  <label>開始時間</label>
  <div class="time-inputs">
    <input id="secondStartHour" type="number" min="0" max="23" placeholder="時" />
    <input id="secondStartMinute" type="number" min="0" max="59" placeholder="分" />
    <input id="secondStartSecond" type="number" min="0" max="59" placeholder="秒" />
  </div>

  <label>結束時間</label>
  <div class="time-inputs">
    <input id="secondEndHour" type="number" min="0" max="23" placeholder="時" />
    <input id="secondEndMinute" type="number" min="0" max="59" placeholder="分" />
    <input id="secondEndSecond" type="number" min="0" max="59" placeholder="秒" />
  </div>

  <label>包裹數量</label>
  <input id="secondQuantity" type="number" min="1" placeholder="輸入包裹數量" />

  <button id="calcSecond">計算第二次上架平均秒數</button>
  <div class="result" id="resultSecond">尚未計算</div>
</fieldset>

<button id="calcAverage">計算兩次上架平均秒數</button>
<div class="result" id="resultAverage">尚未計算</div>

<script>
function getTimeInSeconds(h, m, s) {
  return h*3600 + m*60 + s;
}

function validateInputs(h1, m1, s1, h2, m2, s2, qty) {
  if ([h1,m1,s1,h2,m2,s2,qty].some(v => v === '' || isNaN(v))) return false;
  if (qty <= 0) return false;
  if (h1<0 || h1>23 || h2<0 || h2>23) return false;
  if (m1<0 || m1>59 || m2<0 || m2>59) return false;
  if (s1<0 || s1>59 || s2<0 || s2>59) return false;
  return true;
}

let firstAvg = null;
let secondAvg = null;

document.getElementById('calcFirst').addEventListener('click', () => {
  const h1 = Number(document.getElementById('firstStartHour').value);
  const m1 = Number(document.getElementById('firstStartMinute').value);
  const s1 = Number(document.getElementById('firstStartSecond').value);
  const h2 = Number(document.getElementById('firstEndHour').value);
  const m2 = Number(document.getElementById('firstEndMinute').value);
  const s2 = Number(document.getElementById('firstEndSecond').value);
  const qty = Number(document.getElementById('firstQuantity').value);

  if (!validateInputs(h1,m1,s1,h2,m2,s2,qty)) {
    alert('請輸入有效的第一次上架時間與包裹數量！');
    return;
  }

  let startSec = getTimeInSeconds(h1,m1,s1);
  let endSec = getTimeInSeconds(h2,m2,s2);
  if (endSec < startSec) endSec += 24*3600;

  const totalSec = endSec - startSec;
  firstAvg = totalSec / qty;

  document.getElementById('resultFirst').textContent = `第一次上架平均秒數：${firstAvg.toFixed(2)} 秒`;
});

document.getElementById('calcSecond').addEventListener('click', () => {
  const h1 = Number(document.getElementById('secondStartHour').value);
  const m1 = Number(document.getElementById('secondStartMinute').value);
  const s1 = Number(document.getElementById('secondStartSecond').value);
  const h2 = Number(document.getElementById('secondEndHour').value);
  const m2 = Number(document.getElementById('secondEndMinute').value);
  const s2 = Number(document.getElementById('secondEndSecond').value);
  const qty = Number(document.getElementById('secondQuantity').value);

  if (!validateInputs(h1,m1,s1,h2,m2,s2,qty)) {
    alert('請輸入有效的第二次上架時間與包裹數量！');
    return;
  }

  let startSec = getTimeInSeconds(h1,m1,s1);
  let endSec = getTimeInSeconds(h2,m2,s2);
  if (endSec < startSec) endSec += 24*3600;

  const totalSec = endSec - startSec;
  secondAvg = totalSec / qty;

  document.getElementById('resultSecond').textContent = `第二次上架平均秒數：${secondAvg.toFixed(2)} 秒`;
});

document.getElementById('calcAverage').addEventListener('click', () => {
  if (firstAvg === null || secondAvg === null) {
    alert('請先計算第一次與第二次上架的平均秒數');
    return;
  }
  const avg = (firstAvg + secondAvg) / 2;
  document.getElementById('resultAverage').textContent = `兩次上架平均秒數：${avg.toFixed(2)} 秒`;
});
</script>

</body>
</html>
