# ice<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>빙축열 및 냉동기 운전일지</title>
    <style>
        body { font-family: '맑은 고딕', sans-serif; background-color: #f0f4f8; margin: 0; padding: 15px; color: #333; }
        .container { max-width: 900px; margin: 0 auto; background: #fff; padding: 20px; border-radius: 12px; box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
        h2 { color: #1F497D; text-align: center; margin-bottom: 5px; }
        .subtitle { text-align: center; font-size: 13px; color: #666; margin-bottom: 20px; }
        
        .section-box { background: #f8fafc; border: 1px solid #cbd5e0; padding: 15px; border-radius: 8px; margin-bottom: 15px; }
        .section-title { font-weight: bold; color: #2b6cb0; margin-bottom: 10px; font-size: 15px; border-bottom: 2px solid #2b6cb0; padding-bottom: 5px; }
        
        .form-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 10px; }
        .form-group { display: flex; flex-direction: column; }
        label { font-size: 12px; font-weight: bold; color: #4a5568; margin-bottom: 3px; }
        input, select { padding: 8px; border: 1px solid #cbd5e0; border-radius: 4px; font-size: 14px; }
        
        .btn-group { text-align: center; margin-top: 20px; }
        .btn { background: #3182ce; color: white; border: none; padding: 12px 25px; font-size: 16px; border-radius: 6px; cursor: pointer; font-weight: bold; width: 100%; }
        .btn:hover { background: #2b6cb0; }
        
        table { width: 100%; border-collapse: collapse; margin-top: 25px; font-size: 13px; }
        th, td { border: 1px solid #cbd5e0; padding: 8px; text-align: center; }
        th { background: #1F497D; color: white; }
        .status-normal { color: #2f855a; font-weight: bold; }
        .status-warning { color: #c53030; font-weight: bold; background: #fff5f5; }
    </style>
</head>
<body>

<div class="container">
    <h2>❄️ 빙축열 및 냉동기 운전일지</h2>
    <div class="subtitle">현장 점검 수치 입력 및 실시간 자동 판정 시스템</div>
    
    <form id="logForm" onsubmit="saveLog(event)">
        <!-- 기본 정보 -->
        <div class="section-box">
            <div class="section-title">1. 기본 점검 정보</div>
            <div class="form-grid">
                <div class="form-group">
                    <label>일자</label>
                    <input type="date" id="logDate" required>
                </div>
                <div class="form-group">
                    <label>운전 구분</label>
                    <select id="operationMode">
                        <option value="축냉">축냉운전</option>
                        <option value="단독">단독운전</option>
                        <option value="방냉">방냉운전</option>
                        <option value="병렬">병렬운전</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>냉동기 호기</label>
                    <select id="chillerNo">
                        <option value="1호기">1호기</option>
                        <option value="2호기">2호기</option>
                        <option value="3호기">3호기</option>
                    </select>
                </div>
                <div class="form-group">
                    <label>점검 시간</label>
                    <input type="time" id="logTime" required>
                </div>
            </div>
        </div>

        <!-- 냉동기 주부속 설비 -->
        <div class="section-box">
            <div class="section-title">2. 냉동기 전기 및 압력/온도</div>
            <div class="form-grid">
                <div class="form-group"><label>전압 (kV)</label><input type="number" step="0.001" id="voltage" placeholder="3.300"></div>
                <div class="form-group"><label>전류 (A)</label><input type="number" step="0.1" id="current" placeholder="100.0"></div>
                <div class="form-group"><label>증발온도 (℃)</label><input type="number" step="0.1" id="evapTemp" placeholder="-4.1"></div>
                <div class="form-group"><label>응축온도 (℃)</label><input type="number" step="0.1" id="condTemp" placeholder="35.0"></div>
                <div class="form-group"><label>저압 (kPa)</label><input type="number" step="0.1" id="lowPress" placeholder="-70.0"></div>
                <div class="form-group"><label>고압 (kPa)</label><input type="number" step="0.1" id="highPress" placeholder="45.0"></div>
                <div class="form-group"><label>유압 (SEC)</label><input type="number" step="0.1" id="oilSec" placeholder="2.5"></div>
            </div>
        </div>

        <!-- 냉각수 -->
        <div class="section-box">
            <div class="section-title">3. 냉각수 시스템</div>
            <div class="form-grid">
                <div class="form-group"><label>냉각수 입구온도 (℃)</label><input type="number" step="0.1" id="cwInTemp" placeholder="29.0"></div>
                <div class="form-group"><label>냉각수 출구온도 (℃)</label><input type="number" step="0.1" id="cwOutTemp" placeholder="35.0"></div>
                <div class="form-group"><label>냉각수 입구압력 (kg/㎠)</label><input type="number" step="0.1" id="cwInPress" placeholder="9.5"></div>
                <div class="form-group"><label>냉각수 출구압력 (kg/㎠)</label><input type="number" step="0.1" id="cwOutPress" placeholder="10.0"></div>
            </div>
        </div>

        <!-- 브라인 -->
        <div class="section-box">
            <div class="section-title">4. 브라인 시스템</div>
            <div class="form-grid">
                <div class="form-group"><label>브라인 입구온도 (℃)</label><input type="number" step="0.1" id="brInTemp" placeholder="2.0"></div>
                <div class="form-group"><label>브라인 출구온도 (℃)</label><input type="number" step="0.1" id="brOutTemp" placeholder="-1.5"></div>
                <div class="form-group"><label>브라인 입구압력 (kg/㎠)</label><input type="number" step="0.1" id="brInPress" placeholder="1.4"></div>
                <div class="form-group"><label>브라인 출구압력 (kg/㎠)</label><input type="number" step="0.1" id="brOutPress" placeholder="0.7"></div>
            </div>
        </div>

        <!-- 외기 환경 -->
        <div class="section-box">
            <div class="section-title">5. 외기 환경 및 기타</div>
            <div class="form-grid">
                <div class="form-group"><label>외기온도 (℃)</label><input type="number" step="0.1" id="outTemp" placeholder="28.0"></div>
                <div class="form-group"><label>외습도 (%)</label><input type="number" step="0.1" id="outHum" placeholder="60.0"></div>
            </div>
        </div>

        <div class="btn-group">
            <button type="submit" class="btn">💾 운전일지 기록 저장</button>
        </div>
    </form>

    <!-- 기록된 데이터 목록 -->
    <h3>📋 오늘 입력된 점검 내역</h3>
    <table id="logTable">
        <thead>
            <tr>
                <th>시간</th>
                <th>호기</th>
                <th>구분</th>
                <th>전류(A)</th>
                <th>고압(kPa)</th>
                <th>상태 판정</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td colspan="6" style="color:#888;">입력된 데이터가 없습니다.</td>
            </tr>
        </tbody>
    </table>
</div>

<script>
    // 오늘 날짜 자동 세팅
    document.getElementById('logDate').valueAsDate = new Date();

    function saveLog(event) {
        event.preventDefault();
        
        const time = document.getElementById('logTime').value || "23:00";
        const chiller = document.getElementById('chillerNo').value;
        const mode = document.getElementById('operationMode').value;
        const current = document.getElementById('current').value || "-";
        const highPress = parseFloat(document.getElementById('highPress').value) || 0;

        // 자동 시스템 상태 판정 (빨간색 글씨 대신 명확한 상태 문구 출력)
        let statusHtml = '<span class="status-normal">✅ 정상</span>';
        if (highPress > 65 || current > 108) {
            statusHtml = '<span class="status-warning">⚠️ 수치 주의</span>';
        }

        const tbody = document.querySelector('#logTable tbody');
        if (tbody.querySelector('td[colspan]')) {
            tbody.innerHTML = ''; // 초기 안내문구 제거
        }

        const row = document.createElement('tr');
        row.innerHTML = `
            <td>${time}</td>
            <td>${chiller}</td>
            <td>${mode}</td>
            <td>${current}</td>
            <td>${highPress}</td>
            <td>${statusHtml}</td>
        `;
        tbody.prepend(row);

        alert("운전일지가 정상적으로 기록되었습니다!");
        document.getElementById('logForm').reset();
        document.getElementById('logDate').valueAsDate = new Date();
    }
</script>

</body>
</html>
