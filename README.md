<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>TRPG용 서버</title>
  <!-- Bootstrap CSS -->
  <link href="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/css/bootstrap.min.css" rel="stylesheet">
  <style>
    body {
      font-family: Arial, sans-serif;
      padding: 20px;
    }
    /* Stat bars */
    .stat {
      margin-bottom: 20px;
    }
    .progress {
      height: 30px;
      width: 200px; /* Set all bars to 200px width */
    }
    /* Button margins */
    .btn-group {
      margin-right: 10px;
    }
    /* Hidden section for stat configuration */
    .hidden {
      display: none;
    }
  </style>
</head>
<body>

  <!-- Navbar using Bootstrap -->
  <nav class="navbar navbar-dark bg-dark">
    <a class="navbar-brand" href="#">TRPG용 서버</a>
  </nav>

  <!-- Random Number Buttons -->
  <div class="mt-4">
    <button id="roll1to9Btn" class="btn btn-primary mr-3">1 ~ 9 숫자 출력</button>
    <span id="roll1to9Result">결과: </span>
    <br><br>
    <button id="roll1to20Btn" class="btn btn-primary mr-3">1 ~ 20 숫자 출력</button>
    <span id="roll1to20Result">결과: </span>
  </div>

  <!-- Stat Bars and Inputs -->
  <div class="mt-4">
    <!-- 공격력 -->
    <div class="stat">
      <label>공격력:</label>
      <div class="btn-group">
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('atk', -1)">-1</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('atk', 1)">+1</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('atk', -5)">-5</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('atk', 5)">+5</button>
      </div>
      <span id="atkValue">0</span>
      <div class="progress mt-2">
        <div id="atkBar" class="progress-bar progress-bar-atk" style="width: 0%; background-color: red;"></div>
      </div>
    </div>

    <!-- 방어력 -->
    <div class="stat">
      <label>방어력:</label>
      <div class="btn-group">
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('def', -1)">-1</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('def', 1)">+1</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('def', -5)">-5</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('def', 5)">+5</button>
      </div>
      <span id="defValue">0</span>
      <div class="progress mt-2">
        <div id="defBar" class="progress-bar progress-bar-def" style="width: 0%; background-color: blue;"></div>
      </div>
    </div>

    <!-- 주력량 (Max 500) -->
    <div class="stat">
      <label>주력량:</label>
      <div class="btn-group">
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('agi', -5)">-5</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('agi', 5)">+5</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('agi', -50)">-50</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('agi', 50)">+50</button>
      </div>
      <span id="agiValue">0</span>
      <div class="progress mt-2">
        <div id="agiBar" class="progress-bar progress-bar-agi" style="width: 0%; background-color: skyblue;"></div>
      </div>
    </div>

    <!-- 체력 -->
    <div class="stat">
      <label>체력:</label>
      <div class="btn-group">
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('hp', -1)">-1</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('hp', 1)">+1</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('hp', -5)">-5</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('hp', 5)">+5</button>
      </div>
      <span id="hpValue">0</span>
      <div class="progress mt-2">
        <div id="hpBar" class="progress-bar progress-bar-hp" style="width: 0%; background-color: green;"></div>
      </div>
    </div>

    <!-- 재능 -->
    <div class="stat">
      <label>재능:</label>
      <div class="btn-group">
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('talent', -1)">-1</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('talent', 1)">+1</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('talent', -5)">-5</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('talent', 5)">+5</button>
      </div>
      <span id="talentValue">0</span>
      <div class="progress mt-2">
        <div id="talentBar" class="progress-bar progress-bar-talent" style="width: 0%; background-color: purple;"></div>
      </div>
    </div>

    <!-- 지능 -->
    <div class="stat">
      <label>지능:</label>
      <div class="btn-group">
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('intelligence', -1)">-1</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('intelligence', 1)">+1</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('intelligence', -5)">-5</button>
        <button class="btn btn-secondary btn-sm" onclick="adjustStat('intelligence', 5)">+5</button>
      </div>
      <span id="intelligenceValue">0</span>
      <div class="progress mt-2">
        <div id="intelligenceBar" class="progress-bar" style="width: 0%; background-color: lightgreen;"></div>
      </div>
    </div>
  </div>

  <!-- 새로운 스탯 설정 -->
  <div class="mt-4">
    <button id="newStatBtn" class="btn btn-primary">새로운 스탯 설정</button>
  </div>

  <!-- 스탯 설정 폼 -->
  <div id="statConfigForm" class="hidden mt-4">
    <h4>스탯 설정</h4>
    <div class="form-group">
      <label for="statName">스탯 이름</label>
      <input type="text" class="form-control" id="statName" placeholder="스탯 이름 입력">
    </div>
    <div class="form-group">
      <label for="maxValue">최대 수치</label>
      <input type="number" class="form-control" id="maxValue" placeholder="최대 수치 설정" value="100">
    </div>
    <div class="form-group">
      <label for="statColor">색상 선택</label>
      <input type="color" class="form-control" id="statColor" value="#ff0000">
    </div>
    <button id="createStatBtn" class="btn btn-success">스탯 생성</button>
  </div>

  <!-- 생성된 스탯이 표시될 곳 -->
  <div id="statsContainer" class="mt-4"></div>

  <!-- JavaScript -->
  <script>
    // 주사위 버튼 이벤트
    document.getElementById('roll1to9Btn').addEventListener('click', function() {
      const result = Math.floor(Math.random() * 9) + 1;
      document.getElementById('roll1to9Result').textContent = "결과: " + result;
    });

    document.getElementById('roll1to20Btn').addEventListener('click', function() {
      const result = Math.floor(Math.random() * 20) + 1;
      document.getElementById('roll1to20Result').textContent = "결과: " + result;
    });

    // Stat limits
    const maxValues = {
      atk: 20,
      def: 20,
      agi: 500,
      hp: 20,
      talent: 20,
      intelligence: 20
    };

    // Adjust stat values and update the progress bars
    function adjustStat(stat, amount) {
      const valueElement = document.getElementById(stat + 'Value');
      let currentValue = parseInt(valueElement.textContent);

      const newValue = Math.min(maxValues[stat], Math.max(0, currentValue + amount));
      valueElement.textContent = newValue;

      let percentage;
      if (stat === 'agi') {
        percentage = (newValue / 500) * 100; // For 주력량, max value is 500
      } else {
        percentage = (newValue / 20) * 100; // For others, max value is 20
      }

      const progressBar = document.getElementById(stat + 'Bar');
      progressBar.style.width = percentage + '%';
    }

    // 새로운 스탯 설정 폼
    const newStatBtn = document.getElementById('newStatBtn');
    const statConfigForm = document.getElementById('statConfigForm');

    newStatBtn.addEventListener('click', function() {
      statConfigForm.classList.toggle('hidden');
    });

    // "스탯 생성" 버튼 클릭 이벤트
    document.getElementById('createStatBtn').addEventListener('click', function() {
      const statName = document.getElementById('statName').value;
      const maxValue = parseInt(document.getElementById('maxValue').value);
      const statColor = document.getElementById('statColor').value;

      if (!statName || isNaN(maxValue) || maxValue <= 0) {
        alert('올바른 스탯 이름과 최대 수치를 입력하세요.');
        return;
      }

      createNewStat(statName, maxValue, statColor);
      
      document.getElementById('statName').value = '';
      document.getElementById('maxValue').value = 100;
      statConfigForm.classList.add('hidden');
    });

    // 동적으로 새로운 스탯 생성 함수
    function createNewStat(name, max, color) {
      const statsContainer = document.getElementById('statsContainer');

      const statElement = document.createElement('div');
      statElement.classList.add('stat');
      statElement.innerHTML = `
        <label>${name}:</label>
        <div class="btn-group">
          <button class="btn btn-secondary btn-sm" onclick="adjustStatDynamic(this, ${max}, -1)">-1</button>
          <button class="btn btn-secondary btn-sm" onclick="adjustStatDynamic(this, ${max}, 1)">+1</button>
          <button class="btn btn-secondary btn-sm" onclick="adjustStatDynamic(this, ${max}, ${-Math.floor(max * 0.1)})">-${Math.floor(max * 0.1)}</button>
          <button class="btn btn-secondary btn-sm" onclick="adjustStatDynamic(this, ${max}, ${Math.floor(max * 0.1)})">+${Math.floor(max * 0.1)}</button>
          <button class="btn btn-secondary btn-sm" onclick="adjustStatDynamic(this, ${max}, ${-Math.floor(max * 0.5)})">-${Math.floor(max * 0.5)}</button>
          <button class="btn btn-secondary btn-sm" onclick="adjustStatDynamic(this, ${max}, ${Math.floor(max * 0.5)})">+${Math.floor(max * 0.5)}</button>
        </div>
        <span class="statValue">0</span> / ${max}
        <div class="progress mt-2">
          <div class="progress-bar" style="width: 0%; background-color: ${color};"></div>
        </div>
      `;
      statsContainer.appendChild(statElement);
    }

    // 새로운 동적 스탯 조정 함수
    function adjustStatDynamic(button, max, amount) {
      const statContainer = button.parentElement.parentElement;
      const valueElement = statContainer.querySelector('.statValue');
      const progressBar = statContainer.querySelector('.progress-bar');

      let currentValue = parseInt(valueElement.textContent);
      const newValue = Math.min(max, Math.max(0, currentValue + amount));

      valueElement.textContent = newValue;
      progressBar.style.width = (newValue / max) * 100 + '%';
    }
  </script>

  <!-- Bootstrap JS and dependencies -->
  <script src="https://code.jquery.com/jquery-3.5.1.slim.min.js"></script>
  <script src="https://cdn.jsdelivr.net/npm/@popperjs/core@2.9.2/dist/umd/popper.min.js"></script>
  <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.5.2/js/bootstrap.min.js"></script>

</body>
</html>
