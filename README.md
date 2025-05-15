<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Package Tracking Simulator</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      max-width: 600px;
      margin: 40px auto;
      padding: 20px;
      background: #f4f4f4;
      color: #333;
    }
    h1 {
      text-align: center;
      margin-bottom: 20px;
    }
    input[type="text"] {
      width: 100%;
      padding: 12px 10px;
      margin-bottom: 20px;
      font-size: 16px;
      border: 1px solid #ccc;
      border-radius: 4px;
    }
    button {
      width: 100%;
      background-color: #0070c9;
      color: white;
      padding: 12px;
      font-size: 16px;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    button:hover {
      background-color: #005fa3;
    }
    .tracking-info {
      margin-top: 20px;
      background: white;
      padding: 20px;
      border-radius: 6px;
      box-shadow: 0 0 8px rgba(0,0,0,0.1);
    }
    .status {
      font-weight: bold;
      font-size: 18px;
      margin-bottom: 15px;
    }
    .timeline {
      list-style: none;
      padding: 0;
    }
    .timeline li {
      padding-left: 20px;
      margin-bottom: 15px;
      position: relative;
      font-size: 14px;
      color: #555;
    }
    .timeline li:before {
      content: "";
      position: absolute;
      left: 0;
      top: 5px;
      width: 10px;
      height: 10px;
      border-radius: 50%;
      background-color: #ccc;
    }
    .timeline li.active:before {
      background-color: #0070c9;
    }
  </style>
</head>
<body>
  <h1>Package Tracking Simulator</h1>
  <input type="text" id="trackingCode" placeholder="Enter Tracking Code" />
  <button onclick="trackPackage()">Track</button>

  <div class="tracking-info" id="trackingInfo" style="display:none;">
    <div class="status" id="status"></div>
    <ul class="timeline" id="timeline"></ul>
  </div>

  <script>
    // Your package tracking data (static for demo)
    const packageData = {
      code: "FEDEX2087865SY",
      origin: "Syria",
      destination: "Av. El Ejército N° 100 Barrio San Pedro - Huacaybamba - Huánuco, Peru",
      startDate: "2025-05-13", // Journey start date
      totalDays: 4,
      checkpoints: [
        { day: 1, location: "Damascus, Syria", arrival: "2025-05-13" },
        { day: 2, location: "Beirut, Lebanon", arrival: "2025-05-14" },
        { day: 3, location: "Lima, Peru", arrival: "2025-05-15" },
        { day: 4, location: "Huacaybamba, Peru", arrival: "2025-05-16" }
      ]
    };

    function trackPackage() {
      const inputCode = document.getElementById("trackingCode").value.trim().toUpperCase();
      const infoDiv = document.getElementById("trackingInfo");
      const statusDiv = document.getElementById("status");
      const timelineUl = document.getElementById("timeline");

      timelineUl.innerHTML = ""; // clear previous
      infoDiv.style.display = "none";

      if (inputCode !== packageData.code) {
        alert("Tracking code not found!");
        return;
      }

      const today = new Date();
      const start = new Date(packageData.startDate);
      const diffTime = today - start;
      const diffDays = Math.floor(diffTime / (1000 * 60 * 60 * 24)) + 1; // day count from startDate

      let currentDay = diffDays;
      if (currentDay > packageData.totalDays) currentDay = packageData.totalDays;

      statusDiv.textContent = `Status: In Transit (Day ${currentDay} of ${packageData.totalDays})`;
      infoDiv.style.display = "block";

      packageData.checkpoints.forEach(checkpoint => {
        const li = document.createElement("li");
        li.textContent = `${checkpoint.location} - Arrival: ${checkpoint.arrival}`;
        if (checkpoint.day <= currentDay) {
          li.classList.add("active");
        }
        timelineUl.appendChild(li);
      });
    }
  </script>
</body>
</html>
