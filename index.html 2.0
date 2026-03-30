<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>College Football NET Rankings</title>
<style>
  body { font-family: Arial, sans-serif; background: #f4f4f4; margin: 0; padding: 0; }
  h1 { text-align: center; background: #111; color: #fff; padding: 10px 0; margin: 0 0 20px 0; }
  .input-row { text-align: center; margin-bottom: 10px; }
  input { margin: 0 5px; padding: 5px; width: 80px; }
  button { padding: 5px 10px; }
  table { width: 90%; margin: auto; border-collapse: collapse; background: #fff; }
  th, td { border: 1px solid #ccc; padding: 5px; text-align: center; }
  th { background: #222; color: #fff; }
  tr.top25 { background: #cce5ff; font-weight: bold; }
</style>
</head>
<body>

<h1>College Football NET Rankings</h1>

<div class="input-row">
  <input type="text" id="teamName" placeholder="Team Name">
  <input type="text" id="wins" placeholder="Wins">
  <input type="text" id="losses" placeholder="Losses">
  <input type="text" id="ypp" placeholder="YPP">
  <input type="text" id="yppa" placeholder="YPPA">
  <input type="text" id="sos" placeholder="SOS (1=hard,136=easy)">
  <button onclick="addOrUpdateTeam()">Add / Update Team</button>
  <button onclick="saveTeams()">Save</button>
</div>

<table id="rankingTable">
  <thead>
    <tr>
      <th>RK</th> <!-- ✅ NEW -->
      <th>TEAM</th>
      <th>W-L</th>
      <th>YPP</th>
      <th>YPPA</th>
      <th>SOS</th>
      <th>NET</th>
    </tr>
  </thead>
  <tbody></tbody>
</table>

<script>
let teams = [];

// Load saved teams
if (localStorage.getItem('cfbTeams')) {
  teams = JSON.parse(localStorage.getItem('cfbTeams'));
  updateTable();
}

// NET calculation function
function calculateNET(wins, losses, ypp, yppa, sos) {
  let winPct = wins / (wins + losses);
  let efficiency = ypp - yppa;
  let efficiencyScore = (efficiency + 3) / 6;
  efficiencyScore = Math.max(0, Math.min(1, efficiencyScore));
  let sosScore = (137 - sos) / 136;
  let net = (winPct * 50) + (efficiencyScore * 30) + (sosScore * 20);
  return Math.round(net);
}

// Add or update a team
function addOrUpdateTeam() {
  let name = document.getElementById('teamName').value.trim();
  let wins = parseFloat(document.getElementById('wins').value);
  let losses = parseFloat(document.getElementById('losses').value);
  let ypp = parseFloat(document.getElementById('ypp').value);
  let yppa = parseFloat(document.getElementById('yppa').value);
  let sos = parseFloat(document.getElementById('sos').value);

  if (!name || isNaN(wins) || isNaN(losses) || isNaN(ypp) || isNaN(yppa) || isNaN(sos)) {
    alert('Please fill all fields with valid numbers.');
    return;
  }

  let existingIndex = teams.findIndex(t => t.name === name);
  let net = calculateNET(wins, losses, ypp, yppa, sos);

  let teamObj = {name, wins, losses, ypp, yppa, sos, net};

  if (existingIndex >= 0) {
    teams[existingIndex] = teamObj;
  } else {
    teams.push(teamObj);
  }

  updateTable();
}

// Save to localStorage
function saveTeams() {
  localStorage.setItem('cfbTeams', JSON.stringify(teams));
  alert('Teams saved!');
}

// Update table and auto-sort
function updateTable() {
  teams.sort((a,b) => b.net - a.net);
  let tbody = document.querySelector('#rankingTable tbody');
  tbody.innerHTML = '';
  teams.forEach((team, i) => {
    let tr = document.createElement('tr');
    if (i < 25) tr.classList.add('top25');
    tr.innerHTML = `
      <td>${i + 1}</td> <!-- ✅ RANK -->
      <td>${team.name}</td>
      <td>${team.wins}-${team.losses}</td>
      <td>${team.ypp}</td>
      <td>${team.yppa}</td>
      <td>${team.sos}</td>
      <td>${team.net}</td>
    `;
    tbody.appendChild(tr);
  });
}
</script>

</body>
</html>
