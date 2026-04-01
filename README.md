# Smart-Hospital-queue-management-
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<title>Smart Hospital Queue</title>

<style>
body {
  margin: 0;
  font-family: 'Segoe UI', sans-serif;
  background: linear-gradient(135deg, #667eea, #764ba2);
  color: white;
}

.container {
  max-width: 800px;
  margin: auto;
  padding: 20px;
}

h1 {
  text-align: center;
  margin-bottom: 20px;
}

.glass {
  background: rgba(255, 255, 255, 0.1);
  padding: 20px;
  border-radius: 15px;
  backdrop-filter: blur(10px);
}

.inputBox {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

input {
  flex: 1;
  padding: 12px;
  border-radius: 10px;
  border: none;
  outline: none;
}

button {
  padding: 12px;
  border: none;
  border-radius: 10px;
  cursor: pointer;
  font-weight: bold;
}

.addBtn {
  background: #00c6ff;
  color: white;
}

.doneBtn {
  background: #00ff94;
}

.deleteBtn {
  background: #ff4d4d;
  color: white;
}

.queue {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.card {
  background: white;
  color: black;
  padding: 15px;
  border-radius: 12px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.status {
  padding: 5px 10px;
  border-radius: 20px;
  font-size: 12px;
}

.waiting {
  background: orange;
  color: white;
}

.done {
  background: green;
  color: white;
}

.empty {
  text-align: center;
  opacity: 0.7;
}
</style>
</head>

<body>

<div class="container">
  <h1>🏥 Smart Hospital Queue</h1>

  <div class="glass">
    <div class="inputBox">
      <input id="nameInput" placeholder="Enter patient name">
      <button class="addBtn" onclick="addPatient()">Add</button>
    </div>

    <div id="queue" class="queue"></div>
  </div>
</div>

<script>
let patients = JSON.parse(localStorage.getItem("patients")) || [];

function saveData() {
  localStorage.setItem("patients", JSON.stringify(patients));
}

function addPatient() {
  let input = document.getElementById("nameInput");
  let name = input.value.trim();

  if (name === "") {
    alert("Enter patient name");
    return;
  }

  let patient = {
    id: Date.now(),
    name: name,
    token: patients.length + 1,
    status: "Waiting"
  };

  patients.push(patient);
  input.value = "";
  saveData();
  render();
}

function markDone(id) {
  patients = patients.map(p =>
    p.id === id ? { ...p, status: "Completed" } : p
  );
  saveData();
  render();
}

function deletePatient(id) {
  patients = patients.filter(p => p.id !== id);
  saveData();
  render();
}

function render() {
  let queue = document.getElementById("queue");

  if (patients.length === 0) {
    queue.innerHTML = '<p class="empty">No patients in queue</p>';
    return;
  }

  queue.innerHTML = "";

  patients.forEach(p => {
    let div = document.createElement("div");
    div.className = "card";

    div.innerHTML = `
      <div>
        <h3>${p.name}</h3>
        <p>Token: ${p.token}</p>
      </div>

      <div style="display:flex; gap:8px; align-items:center;">
        <span class="status ${p.status === "Waiting" ? "waiting" : "done"}">
          ${p.status}
        </span>

        ${p.status === "Waiting" 
          ? `<button class="doneBtn" onclick="markDone(${p.id})">✔</button>` 
          : ""}

        <button class="deleteBtn" onclick="deletePatient(${p.id})">✖</button>
      </div>
    `;

    queue.appendChild(div);
  });
}

render();
</script>

</body>
</html>
