<!DOCTYPE html>
<html lang="da">
<head>
  <meta charset="UTF-8">
  <title>Renters rente i realtid (pr. time)</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      background: #0f172a;
      color: #e5e7eb;
      display: flex;
      justify-content: center;
      align-items: center;
      height: 100vh;
      margin: 0;
    }
    .box {
      background: #111827;
      padding: 40px;
      border-radius: 12px;
      text-align: center;
      box-shadow: 0 10px 30px rgba(0,0,0,0.4);
      width: 360px;
    }
    h1 {
      margin-bottom: 10px;
      font-size: 22px;
      color: #93c5fd;
    }
    .amount {
      font-size: 28px;
      font-weight: bold;
      margin-top: 20px;
      font-family: monospace;
    }
    .meta {
      margin-top: 10px;
      font-size: 13px;
      color: #9ca3af;
    }
    .milestone {
      margin-top: 20px;
      padding: 12px;
      background: #020617;
      border-radius: 8px;
      font-size: 14px;
      color: #a5b4fc;
      min-height: 40px;
    }
    .timer {
      margin-top: 16px;
      font-size: 13px;
      color: #9ca3af;
      font-family: monospace;
    }
  </style>
</head>
<body>
  <div class="box">
    <h1>Renters rente i realtid</h1>

    <div id="amount" class="amount">0,09000000 kr</div>
    <div class="meta">Start: 0,09 kr · Rente: 25% pr. time</div>

    <div id="milestone" class="milestone">
      Venter på første milepæl…
    </div>

    <div id="timer" class="timer">
      Tid: 00:00:00
    </div>
  </div>

  <script>
    const startBelob = 0.09;   // 0,09 kr
    const rentePrTime = 0.25;  // 25% pr. time

    // Milepæle i kroner
    const milestones = [0.10, 0.25, 0.50, 1, 2, 5, 10, 25, 50, 100, 250, 500, 1000];

    let currentMilestoneIndex = -1;

    const startTid = performance.now();
    const amountEl = document.getElementById("amount");
    const milestoneEl = document.getElementById("milestone");
    const timerEl = document.getElementById("timer");

    function formatTime(seconds) {
      const h = Math.floor(seconds / 3600);
      const m = Math.floor((seconds % 3600) / 60);
      const s = Math.floor(seconds % 60);

      const hh = String(h).padStart(2, "0");
      const mm = String(m).padStart(2, "0");
      const ss = String(s).padStart(2, "0");

      return `${hh}:${mm}:${ss}`;
    }

    function opdater() {
      const nu = performance.now();
      const sekunder = (nu - startTid) / 1000;
      const timer = sekunder / 3600;

      // Renters rente: A = A0 * (1 + r)^t
      const belob = startBelob * Math.pow(1 + rentePrTime, timer);

      amountEl.textContent = belob.toLocaleString("da-DK", {
        minimumFractionDigits: 8,
        maximumFractionDigits: 8
      }) + " kr";

      // Opdater timer
      timerEl.textContent = "Tid: " + formatTime(sekunder);

      // Tjek milepæle
      if (currentMilestoneIndex + 1 < milestones.length) {
        const nextMilestone = milestones[currentMilestoneIndex + 1];
        if (belob >= nextMilestone) {
          currentMilestoneIndex++;
          milestoneEl.textContent =
            "🎯 Milepæl nået: " +
            nextMilestone.toLocaleString("da-DK", { minimumFractionDigits: 2, maximumFractionDigits: 2 }) +
            " kr";
        }
      }

      requestAnimationFrame(opdater);
    }

    opdater();
  </script>
</body>
</html>
