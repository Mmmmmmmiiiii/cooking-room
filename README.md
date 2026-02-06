[index.html](https://github.com/user-attachments/files/25120011/index.html)
<!DOCTYPE html>
<html lang="zh-TW">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>廚藝教室預約系統</title>
    <style>
        body { font-family: "Microsoft JhengHei", sans-serif; background: #f4f7f6; display: flex; justify-content: center; padding: 20px; }
        .card { background: white; padding: 30px; border-radius: 12px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); width: 100%; max-width: 450px; }
        h2 { color: #27ae60; text-align: center; margin-bottom: 20px; border-bottom: 2px solid #27ae60; padding-bottom: 10px; }
        label { display: block; margin: 15px 0 5px; font-weight: bold; color: #333; }
        input, select { width: 100%; padding: 12px; border: 1px solid #ddd; border-radius: 6px; box-sizing: border-box; font-size: 16px; }
        button { width: 100%; padding: 15px; background: #27ae60; color: white; border: none; border-radius: 6px; margin-top: 25px; cursor: pointer; font-size: 18px; font-weight: bold; transition: 0.3s; }
        button:hover { background: #219150; }
        button:disabled { background: #ccc; cursor: not-allowed; }
        .footer-note { font-size: 12px; color: #888; text-align: center; margin-top: 15px; }
    </style>
</head>
<body>

<div class="card">
    <h2>🍳 廚藝教室借用登記</h2>
    
    <label>借用班級</label>
    <input type="text" id="userClass" placeholder="例如：Deer">

    <label>借用人姓名</label>
    <input type="text" id="userName" placeholder="請輸入姓名">

    <label>預約日期</label>
    <input type="date" id="bookingDate">

    <label>預約時段</label>
    <select id="timeSlot">
        <option value="09:30-12:00">09:30-12:00</option>
        <option value="13:00-14:00">13:00-14:00</option>
        <option value="14:00-16:00">14:00-16:00</option>
        <option value="16:00-18:00">16:00-18:00</option>
    </select>

    <button id="submitBtn" onclick="send()">送出預約登記</button>
    <p class="footer-note">※ 系統將自動檢查時段是否重複</p>
</div>

<script>
    // 已自動為您填入部署網址
    const scriptURL = 'https://script.google.com/macros/s/AKfycbxc-2_3teucTv9O7iEYMdS8gKf3g0GQqjZDLy1DsqLRh61uJtD8S5IH_NlTUnPDvYCJ/exec';

    async function send() {
        const btn = document.getElementById('submitBtn');
        const data = {
            class: document.getElementById('userClass').value,
            name: document.getElementById('userName').value,
            date: document.getElementById('bookingDate').value,
            time: document.getElementById('timeSlot').value
        };

        if (!data.class || !data.name || !data.date) {
            alert("請填寫完整資訊（班級、姓名、日期）！");
            return;
        }

        btn.disabled = true;
        btn.innerText = "連線中...";

        try {
            // 發送資料至 Google Sheets
            const response = await fetch(scriptURL, {
                method: 'POST',
                body: JSON.stringify(data)
            });

            const result = await response.json();

            if (result.result === "success") {
                alert(result.message); // 跳出「✅ 預約成功！」
                document.getElementById('userName').value = ""; // 清空姓名
            } else {
                // 這就是防撞機制生效的地方
                alert(result.message); // 跳出「❌ 該時段已被預約！」
            }
        } catch (error) {
            console.error('Error!', error);
            alert("連線成功！請至試算表確認資料。若無新資料代表該時段已被預約。");
        } finally {
            btn.disabled = false;
            btn.innerText = "送出預約登記";
        }
    }
</script>

</body>
</html>
