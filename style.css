<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ระบบวัดระดับน้ำและค่า pH พร้อมคำนวณเวลาการใช้น้ำ</title>
    <style>
        body {
            font-family: 'Roboto', sans-serif;
            margin: 20px;
            text-align: center;
            background-color: #F0F8FF;
        }
        .container {
            margin: 20px auto;
            width: 80%;
        }
        .university-logo {
            margin-bottom: 20px;
        }
        .system-name {
            font-size: 24px;
            font-weight: bold;
            margin-bottom: 40px;
        }
        .box {
            border: 2px solid #000000;
            padding: 20px;
            margin-bottom: 20px;
            text-align: center;
        }
        .box h2 {
            font-size: 20px;
            margin-bottom: 10px;
        }
        .box p {
            font-size: 16px;
            margin-bottom: 10px;
        }
        .ph-value, .ultrasonic-value {
            font-size: 20px;
            font-weight: bold;
            color: red;
        }
        .graph {
            width: 100%;
            height: 200px;
            border: 1px solid #000;
            margin: 20px 0;
        }
        .info {
            font-size: 18px;
        }
        .hidden {
            display: none;
        }
    </style>
    <!-- Chart.js -->
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script>
        let distanceData = [];
        let timeLabels = [];
        let currentDate = '';

        function updateValues() {
            fetch('http://192.168.100.248   /')  // เปลี่ยน URL ตาม IP ของ ESP8266
                .then(response => {
                    if (!response.ok) {
                        throw new Error('Network response was not ok');
                    }
                    return response.json();
                })
                .then(data => {
                    // อัปเดตค่า pH
                    document.getElementById('ph-value').innerText = data.ph;

                    // อัปเดตค่าความลึกจาก Ultrasonic sensor และแปลงเป็นเมตร
                    let depth = (400 - parseFloat(data.distance.replace(' cm', ''))) / 100; // แปลงค่าเป็นเมตร
                    document.getElementById('ultrasonic-value').innerText = depth.toFixed(2) + ' m'; // แสดงความลึกเป็นเมตร

                    // คำนวณปริมาณน้ำจากสูตร 1/2 * 56 * ความลึก
                    let volume = (1 / 2) * 56 * depth; // สูตรคำนวณปริมาณน้ำในเมตร
                    document.getElementById('water-volume').innerText = volume.toFixed(2) + ' ลูกบาศก์เมตร'; // แสดงผลปริมาณน้ำ

                    // คำนวณผลลัพธ์จากการหารปริมาณน้ำ
                    let result1 = volume / 10;
                    let result2 = volume / 20;
                    let result3 = volume / 30;

                    // แสดงผลในตาราง
                    document.getElementById('result1').innerText = result1.toFixed(2);
                    document.getElementById('result2').innerText = result2.toFixed(2);
                    document.getElementById('result3').innerText = result3.toFixed(2);

                    // แสดงวันที่ใช้น้ำ
                    currentDate = new Date().toLocaleDateString('th-TH'); // วันที่ในรูปแบบไทย
                    document.getElementById('date1').innerText = currentDate;
                    document.getElementById('date2').innerText = currentDate;
                    document.getElementById('date3').innerText = currentDate;

                    // คำนวณวันที่ใช้น้ำ
                    let date1 = new Date();
                    date1.setDate(date1.getDate() + Math.ceil(result1)); // เพิ่มจำนวนวันที่จาก result1
                    document.getElementById('date1').innerText = date1.toLocaleDateString('th-TH');

                    let date2 = new Date();
                    date2.setDate(date2.getDate() + Math.ceil(result2)); // เพิ่มจำนวนวันที่จาก result2
                    document.getElementById('date2').innerText = date2.toLocaleDateString('th-TH');

                    let date3 = new Date();
                    date3.setDate(date3.getDate() + Math.ceil(result3)); // เพิ่มจำนวนวันที่จาก result3
                    document.getElementById('date3').innerText = date3.toLocaleDateString('th-TH');

                    // อัปเดตข้อมูลกราฟ
                    const currentTime = new Date().toLocaleTimeString();
                    distanceData.push(depth); // ใช้ค่าความลึกที่เป็นตัวเลข
                    timeLabels.push(currentTime);

                    if (distanceData.length > 10) {
                        distanceData.shift();  // ลบข้อมูลเก่าเมื่อเกิน 10 จุด
                        timeLabels.shift();
                    }

                    myChart.update();
                })
                .catch(error => console.error('Error:', error));
        }

        window.onload = function () {
            // สร้างกราฟเมื่อหน้าเว็บโหลดเสร็จ
            var ctx = document.getElementById('distanceChart').getContext('2d');
            window.myChart = new Chart(ctx, {
                type: 'line',
                data: {
                    labels: timeLabels,
                    datasets: [{
                        label: 'ความลึก (m)',
                        data: distanceData,
                        borderColor: 'rgba(75, 192, 192, 1)',
                        backgroundColor: 'rgba(75, 192, 192, 0.2)',
                        fill: true
                    }]
                },
                options: {
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });

            // แสดงวันที่ปัจจุบัน
            const today = new Date().toLocaleDateString('th-TH'); // วันที่ในรูปแบบไทย
            document.getElementById('current-date').innerText = today;

            // เริ่มอัปเดตข้อมูลทุก 5 วินาที
            updateValues();
            setInterval(updateValues, 5000);
        };
    </script>
</head>
<body>
    <div class="container">
        <div class="university-logo">
            <img src="Ratchaphat.png" width ="200">
        </div>
        <div class="system-name">
            มหาวิทยาลัยราชภัฎเพชรบุรี
        </div>
        <div class="system-name">
            ระบบวัดระดับน้ำและค่า pH พร้อมคำนวณเวลาการใช้น้ำ
        </div>

        <div class="box">
            <h2>แสดงการวัดค่า pH sensor</h2>
            <p>ค่า pH 0-6.9 มีค่าเป็นกรด</p>
            <p>ค่า pH 7.1-14 มีค่าเป็นด่าง</p>
            <div class="ph-value">ค่า pH ที่วัดได้: <span id="ph-value">กำลังโหลด...</span></div>
        </div>

        <div class="box">
            <h2>แสดงการวัดค่า Ultrasonic sensor</h2>
            <div class="info">ความลึกที่วัดได้: <span id="ultrasonic-value">กำลังโหลด...</span></div>
            <canvas id="distanceChart" width="400" height="200"></canvas>  <!-- กราฟแสดงความลึก -->
            <p class="info">ปริมาณน้ำจากการคำนวณ: <span id="water-volume">กำลังคำนวณ...</span></p>
            <p class="info">วันที่วัดค่า: <span id="current-date">กำลังโหลด...</span></p>
        </div>

        <div class="box">
            <h2>เหตุการณ์สมมุติ</h2>
            <table border="1" cellpadding="10" cellspacing="0" style="width: 100%; margin: 0 auto;">
                <tr>
                    <th>หากใช้น้ำ</th>
                    <th class="hidden">ค่าที่วัดได้/ใช้น้ำ</th>
                    <th>น้ำสามารถใช้ได้ถึงวันที่</th>
                </tr>
                <tr>
                    <td>10 ลบ.ม.</td>
                    <td id="result1" class="hidden">กำลังโหลด...</td>
                    <td id="date1">กำลังโหลด...</td>
                </tr>
                <tr>
                    <td>20 ลบ.ม.</td>
                    <td id="result2" class="hidden">กำลังโหลด...</td>
                    <td id="date2">กำลังโหลด...</td>
                </tr>
                <tr>
                    <td>30 ลบ.ม.</td>
                    <td id="result3" class="hidden">กำลังโหลด...</td>
                    <td id="date3">กำลังโหลด...</td>  <!-- เพิ่ม ID สำหรับวันที่ -->
                </tr>
            </table>
        </div>

    </div>
</body> 
</html>


