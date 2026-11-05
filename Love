<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Memoi & Griffins</title>
  <style>
    * { box-sizing: border-box; }
    body {
      margin: 0;
      min-height: 100vh;
      padding: 24px 16px;
      background: #08080d;
      color: #f7f2f5;
      font-family: system-ui, -apple-system, "Segoe UI", sans-serif;
    }
    main { width: 100%; max-width: 520px; margin: auto; }
    header { text-align: center; margin: 14px 0 24px; }
    h1 { margin: 0; font-size: 30px; }
    header p { color: #b8aeb5; margin: 8px 0 0; }
    h2 { margin: 0 0 10px; color: #ff6b9d; font-size: 15px; }
    .card {
      margin-bottom: 14px;
      padding: 20px;
      border: 1px solid #29232b;
      border-radius: 18px;
      background: #141218;
      box-shadow: 0 8px 24px #0006;
    }
    .number { margin: 4px 0; font-size: 48px; font-weight: 800; }
    .detail { color: #c3bac1; font-size: 14px; }
    .row { display: grid; grid-template-columns: 1fr 1fr; gap: 12px; }
    .row .card { min-width: 0; }
    .row .number { font-size: 32px; }
    label { display: block; margin-top: 12px; color: #d3cbd1; font-size: 13px; }
    input {
      width: 100%;
      margin-top: 6px;
      padding: 11px;
      border: 1px solid #403641;
      border-radius: 10px;
      background: #0c0a0e;
      color: white;
      font: inherit;
    }
    button {
      width: 100%;
      margin-top: 14px;
      padding: 12px;
      border: 0;
      border-radius: 11px;
      background: #ff4f8b;
      color: white;
      font: inherit;
      font-weight: 700;
      cursor: pointer;
    }
    .progress {
      height: 9px;
      margin-top: 14px;
      overflow: hidden;
      border-radius: 20px;
      background: #332b34;
    }
    .progress div {
      width: 0;
      height: 100%;
      border-radius: inherit;
      background: #ff4f8b;
      transition: width .3s;
    }
    .note, footer {
      color: #a99fa7;
      font-size: 12px;
      line-height: 1.5;
    }
    .note { margin: 12px 0 0; }
    footer { padding: 8px; text-align: center; }
    @media (max-width: 360px) {
      .row { grid-template-columns: 1fr; gap: 0; }
    }
  </style>
</head>
<body>
  <main>
    <header>
      <h1>Memoi ❤️ Griffins</h1>
      <p>A little countdown made with love</p>
    </header>

    <section class="card">
      <h2>DAYS SINCE WE MET</h2>
      <div class="number" id="daysTogether">—</div>
      <div class="detail">Since Monday, November 2, 2026</div>
    </section>

    <div class="row">
      <section class="card">
        <h2>MEMOI'S BIRTHDAY MONTH 🎂</h2>
        <div class="number" id="aprilCountdown">—</div>
        <div class="detail" id="aprilDate">Until April</div>
      </section>
      <section class="card">
        <h2>GRIFFINS' BIRTHDAY MONTH 🎂</h2>
        <div class="number" id="decemberCountdown">—</div>
        <div class="detail" id="decemberDate">Until December</div>
      </section>
    </div>

    <section class="card">
      <h2>PERIOD COUNTDOWN 🌸</h2>
      <div class="number" id="periodCountdown">—</div>
      <div class="detail" id="nextPeriodDate">Estimated next period</div>
      <div class="progress" aria-label="Estimated cycle progress">
        <div id="cycleProgress"></div>
      </div>
      <div class="detail" id="cycleDay" style="margin-top:8px"></div>
    </section>

    <section class="card">
      <h2>UPDATE CYCLE</h2>
      <label for="lastPeriod">First day of the last period</label>
      <input id="lastPeriod" type="date">

      <label for="cycleLength">Usual cycle length in days</label>
      <input id="cycleLength" type="number" min="15" max="60" value="28">

      <button id="saveCycle" type="button">Save cycle details</button>
      <p class="note">
        Period predictions are estimates and cycles can vary. This countdown
        should not be used as birth control or as a way to predict pregnancy.
      </p>
    </section>

    <footer>Memoi & Griffins • Made with love ❤️</footer>
  </main>

  <script>
    // Date you met: Monday, November 2, 2026.
    const metDate = new Date(2026, 10, 2);

    // Starting estimate from your message:
    // November 5, 2026 + 7 days = November 12, 2026.
    // With a 28-day cycle, that corresponds to an October 15 start date.
    const defaultLastPeriod = new Date(2026, 9, 15);
    const DAY_MS = 24 * 60 * 60 * 1000;

    const $ = (id) => document.getElementById(id);

    function localDate(year, month, day) {
      return new Date(year, month, day);
    }

    function dayNumber(date) {
      return Date.UTC(date.getFullYear(), date.getMonth(), date.getDate()) / DAY_MS;
    }

    function daysBetween(from, to) {
      return Math.round(dayNumber(to) - dayNumber(from));
    }

    function dateInputValue(date) {
      const year = date.getFullYear();
      const month = String(date.getMonth() + 1).padStart(2, "0");
      const day = String(date.getDate()).padStart(2, "0");
      return `${year}-${month}-${day}`;
    }

    function formatDate(date) {
      return date.toLocaleDateString(undefined, {
        month: "long",
        day: "numeric",
        year: "numeric"
      });
    }

    function countdownToMonth(monthIndex) {
      const today = new Date();
      today.setHours(0, 0, 0, 0);

      let target = localDate(today.getFullYear(), monthIndex, 1);
      if (target < today) {
        target = localDate(today.getFullYear() + 1, monthIndex, 1);
      }

      const months =
        (target.getFullYear() - today.getFullYear()) * 12 +
        target.getMonth() - today.getMonth();

      return { months, target };
    }

    function updateBirthdayCountdowns() {
      const april = countdownToMonth(3);
      const december = countdownToMonth(11);

      $("aprilCountdown").textContent =
        `${april.months} ${april.months === 1 ? "month" : "months"}`;
      $("aprilDate").textContent = `Until ${formatDate(april.target)}`;

      $("decemberCountdown").textContent =
        `${december.months} ${december.months === 1 ? "month" : "months"}`;
      $("decemberDate").textContent = `Until ${formatDate(december.target)}`;
    }

    function readSavedCycle() {
      let lastPeriod = defaultLastPeriod;
      let cycleLength = 28;

      try {
        const savedDate = localStorage.getItem("memoiLastPeriod");
        const savedLength = Number(localStorage.getItem("memoiCycleLength"));

        if (savedDate) {
          const [year, month, day] = savedDate.split("-").map(Number);
          lastPeriod = localDate(year, month - 1, day);
        }
        if (Number.isInteger(savedLength) && savedLength >= 15 && savedLength <= 60) {
          cycleLength = savedLength;
        }
      } catch {
        // Use the default values if browser storage is unavailable.
      }

      return { lastPeriod, cycleLength };
    }

    function renderCycle() {
      const { lastPeriod, cycleLength } = readSavedCycle();
      const today = new Date();
      today.setHours(0, 0, 0, 0);

      // Move forward by full cycles until the next predicted period is today or later.
      let nextPeriod = new Date(lastPeriod);
      while (nextPeriod < today) {
        nextPeriod.setDate(nextPeriod.getDate() + cycleLength);
      }

      const cycleStart = new Date(nextPeriod);
      cycleStart.setDate(cycleStart.getDate() - cycleLength);

      const daysLeft = daysBetween(today, nextPeriod);
      const currentCycleDay = Math.max(1, daysBetween(cycleStart, today) + 1);
      const progress = Math.min(100, ((currentCycleDay - 1) / cycleLength) * 100);

      $("periodCountdown").textContent =
        daysLeft === 0 ? "Today" : `${daysLeft} ${daysLeft === 1 ? "day" : "days"}`;
      $("nextPeriodDate").textContent = `Estimated date: ${formatDate(nextPeriod)}`;
      $("cycleDay").textContent = `Estimated cycle day ${currentCycleDay} of ${cycleLength}`;
      $("cycleProgress").style.width = `${progress}%`;

      $("lastPeriod").value = dateInputValue(lastPeriod);
      $("cycleLength").value = cycleLength;
    }

    $("saveCycle").addEventListener("click", () => {
      const dateValue = $("lastPeriod").value;
      const length = Number($("cycleLength").value);

      if (!dateValue) {
        alert("Please enter the first day of the last period.");
        return;
      }
      if (!Number.isInteger(length) || length < 15 || length > 60) {
        alert("Enter a cycle length from 15 to 60 days.");
        return;
      }

      try {
        localStorage.setItem("memoiLastPeriod", dateValue);
        localStorage.setItem("memoiCycleLength", String(length));
      } catch {
        // The countdown still updates for this visit if storage is unavailable.
      }
      renderCycle();
    });

    function render() {
      const today = new Date();
      today.setHours(0, 0, 0, 0);

      $("daysTogether").textContent = Math.max(0, daysBetween(metDate, today));
      updateBirthdayCountdowns();
      renderCycle();
    }

    render();
    setInterval(render, 60 * 60 * 1000);
  </script>
</body>
</html>
