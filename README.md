# Plant-For-Earth-

<html lang="bn">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>🌱 Plant for Earth</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: #e6ffe6;
      margin: 0;
      padding: 0;
      color: #2e7d32;
    }
    header {
      background: #2e7d32;
      color: white;
      padding: 20px;
      text-align: center;
    }
    main {
      max-width: 700px;
      margin: 20px auto;
      background: white;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 0 10px #ccc;
    }
    input, button {
      width: 100%;
      padding: 10px;
      margin: 8px 0;
      border-radius: 6px;
      border: 1px solid #ccc;
      box-sizing: border-box;
    }
    button {
      background: #2e7d32;
      color: white;
      font-weight: bold;
      cursor: pointer;
    }
    ul {
      list-style: none;
      padding-left: 0;
    }
    li {
      background: #f1f8e9;
      padding: 10px;
      margin: 10px 0;
      border-left: 5px solid #2e7d32;
    }
    .actions {
      display: flex;
      gap: 10px;
      margin-top: 10px;
      flex-wrap: wrap;
    }
    .actions button {
      flex: 1;
    }
  </style>
</head>
<body>

  <header>
    <h1>🌱 Plant for Earth</h1>
    <p>আপনার রোপণ করা গাছের তথ্য যুক্ত করুন, সংরক্ষণ করুন ও শেয়ার করুন!</p>
  </header>

  <main>
    <h2>গাছ রোপণের তথ্য</h2>
    <input type="text" id="treeName" placeholder="গাছের নাম (যেমন: আমগাছ)" required />
    <input type="text" id="location" placeholder="লোকেশন (যেমন: ঢাকা, বাংলাদেশ)" required />
    <input type="date" id="plantDate" required />
    <button onclick="addPlant()">✅ গাছ যুক্ত করুন</button>

    <div class="actions">
      <button onclick="printData()">🖨️ প্রিন্ট করুন</button>
      <button onclick="downloadCSV()">⬇️ CSV ডাউনলোড করুন</button>
      <button onclick="clearData()">🗑️ সব মুছুন</button>
    </div>

    <h2>রোপিত গাছের তালিকা 🌳</h2>
    <ul id="plantList"></ul>
  </main>

  <script>
    const plantList = document.getElementById("plantList");

    function loadPlants() {
      const data = JSON.parse(localStorage.getItem("plants")) || [];
      plantList.innerHTML = "";
      data.forEach((plant, index) => {
        const li = document.createElement("li");
        li.innerHTML = `<strong>${plant.name}</strong><br/>
                        📍 ${plant.location}<br/>
                        📅 ${plant.date}`;
        plantList.appendChild(li);
      });
    }

    function addPlant() {
      const name = document.getElementById("treeName").value.trim();
      const location = document.getElementById("location").value.trim();
      const date = document.getElementById("plantDate").value;

      if (!name || !location || !date) {
        alert("সব ফিল্ড পূরণ করুন");
        return;
      }

      const newPlant = { name, location, date };
      const plants = JSON.parse(localStorage.getItem("plants")) || [];
      plants.push(newPlant);
      localStorage.setItem("plants", JSON.stringify(plants));

      document.getElementById("treeName").value = "";
      document.getElementById("location").value = "";
      document.getElementById("plantDate").value = "";

      loadPlants();
    }

    function printData() {
      const printWindow = window.open('', '', 'height=500,width=800');
      printWindow.document.write('<html><head><title>গাছ রোপণের তালিকা</title></head><body>');
      printWindow.document.write('<h1>🌱 রোপণ করা গাছের তালিকা</h1><ul>');
      const data = JSON.parse(localStorage.getItem("plants")) || [];
      data.forEach(plant => {
        printWindow.document.write(`<li><strong>${plant.name}</strong><br/>📍 ${plant.location}<br/>📅 ${plant.date}</li>`);
      });
      printWindow.document.write('</ul></body></html>');
      printWindow.document.close();
      printWindow.print();
    }

    function downloadCSV() {
      const data = JSON.parse(localStorage.getItem("plants")) || [];
      if (data.length === 0) {
        alert("তথ্য খালি");
        return;
      }
      let csv = "Tree Name,Location,Date\n";
      data.forEach(p => {
        csv += `"${p.name}","${p.location}","${p.date}"\n`;
      });

      const blob = new Blob([csv], { type: 'text/csv' });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      a.href = url;
      a.download = "plant-data.csv";
      document.body.appendChild(a);
      a.click();
      document.body.removeChild(a);
    }

    function clearData() {
      if (confirm("আপনি কি নিশ্চিতভাবে সব তথ্য মুছতে চান?")) {
        localStorage.removeItem("plants");
        loadPlants();
      }
    }

    window.onload = loadPlants;
  </script>

</body>
</html>
