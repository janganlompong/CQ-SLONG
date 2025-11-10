<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Penghitung Skor Remi</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f0f0f5;
      margin: 0;
      padding: 20px;
      color: #333;
    }

    h1 {
      text-align: center;
      color: #2c3e50;
    }

    #playerForm, #scoreboard {
      max-width: 600px;
      margin: 20px auto;
      background: #fff;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 2px 6px rgba(0,0,0,0.1);
    }

    input, button {
      padding: 10px;
      margin: 5px;
      border-radius: 5px;
      border: 1px solid #ccc;
    }

    button {
      cursor: pointer;
      background-color: #2ecc71;
      color: white;
      border: none;
    }

    button:hover {
      background-color: #27ae60;
    }

    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 10px;
    }

    th, td {
      border: 1px solid #ddd;
      padding: 8px;
      text-align: center;
    }

    th {
      background-color: #2ecc71;
      color: white;
    }

    #roundInput input {
      width: 80px;
    }

    .reset-btn {
      background-color: #e74c3c;
    }

    .reset-btn:hover {
      background-color: #c0392b;
    }
  </style>
</head>
<body>
  <h1>Penghitung Skor Remi</h1>

  <div id="playerForm">
    <h3>Masukkan Nama Pemain</h3>
    <input type="text" id="playerName" placeholder="Nama pemain" />
    <button onclick="addPlayer()">Tambah Pemain</button>
    <button class="reset-btn" onclick="resetGame()">Reset Permainan</button>
    <ul id="playerList"></ul>
  </div>

  <div id="scoreboard" style="display:none;">
    <h3>Tabel Skor</h3>
    <table id="scoreTable">
      <thead>
        <tr id="playerHeader">
          <th>Ronde</th>
        </tr>
      </thead>
      <tbody id="scoreBody"></tbody>
      <tfoot>
        <tr id="totalRow">
          <th>Total</th>
        </tr>
      </tfoot>
    </table>

    <h3>Tambah Skor Ronde</h3>
    <div id="roundInput"></div>
    <button onclick="addRound()">Tambah Ronde</button>
  </div>

  <script>
    let players = [];
    let rounds = [];

    function addPlayer() {
      const name = document.getElementById('playerName').value.trim();
      if (!name) return alert('Nama pemain tidak boleh kosong!');
      players.push(name);
      document.getElementById('playerName').value = '';
      updatePlayerList();
      if (players.length > 0) document.getElementById('scoreboard').style.display = 'block';
      renderTableHeader();
    }

    function updatePlayerList() {
      const list = document.getElementById('playerList');
      list.innerHTML = '';
      players.forEach(p => {
        const li = document.createElement('li');
        li.textContent = p;
        list.appendChild(li);
      });
    }

    function renderTableHeader() {
      const header = document.getElementById('playerHeader');
      header.innerHTML = '<th>Ronde</th>';
      players.forEach(p => {
        const th = document.createElement('th');
        th.textContent = p;
        header.appendChild(th);
      });

      const total = document.getElementById('totalRow');
      total.innerHTML = '<th>Total</th>';
      players.forEach(() => {
        const td = document.createElement('td');
        td.textContent = '0';
        total.appendChild(td);
      });

      const roundInput = document.getElementById('roundInput');
      roundInput.innerHTML = '';
      players.forEach(p => {
        const input = document.createElement('input');
        input.type = 'number';
        input.placeholder = p;
        roundInput.appendChild(input);
      });
    }

    function addRound() {
      const inputs = document.querySelectorAll('#roundInput input');
      const scores = Array.from(inputs).map(i => Number(i.value) || 0);
      rounds.push(scores);
      renderTable();
      inputs.forEach(i => i.value = '');
    }

    function renderTable() {
      const tbody = document.getElementById('scoreBody');
      tbody.innerHTML = '';
      rounds.forEach((r, i) => {
        const tr = document.createElement('tr');
        const tdRound = document.createElement('td');
        tdRound.textContent = i + 1;
        tr.appendChild(tdRound);
        r.forEach(score => {
          const td = document.createElement('td');
          td.textContent = score;
          tr.appendChild(td);
        });
        tbody.appendChild(tr);
      });
      updateTotal();
    }

    function updateTotal() {
      const totalRow = document.getElementById('totalRow');
      const totals = new Array(players.length).fill(0);
      rounds.forEach(r => r.forEach((score, i) => totals[i] += score));
      totalRow.innerHTML = '<th>Total</th>';
      totals.forEach(t => {
        const td = document.createElement('td');
        td.textContent = t;
        totalRow.appendChild(td);
      });
    }

    function resetGame() {
      if (!confirm('Yakin ingin reset permainan?')) return;
      players = [];
      rounds = [];
      document.getElementById('playerList').innerHTML = '';
      document.getElementById('scoreBody').innerHTML = '';
      document.getElementById('scoreboard').style.display = 'none';
    }
  </script>
</body>
</html>
