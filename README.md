# thisisamassivevirusdontclick
this is a virus safe your computer dont download<!doctype html>
<html lang="hu">
<head>
  <meta charset="utf-8" />
  <title>thisisamassivevirusdontclick</title>
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <style>
    :root { --bg:#0b1220; --card:#071226; --accent:#e04646; --muted:#9aa6b2; --glass: rgba(255,255,255,0.03); }
    body { margin:0; font-family: system-ui, -apple-system, Roboto, Arial; background:linear-gradient(180deg,#071026 0%, #071226 100%); color:#e6eef6; min-height:100vh; display:flex; align-items:center; justify-content:center; padding:2rem; }
    .card { width:720px; border-radius:12px; padding:1.25rem; background:linear-gradient(180deg, rgba(255,255,255,0.02), rgba(255,255,255,0.01)); box-shadow:0 10px 40px rgba(2,6,23,0.7); border:1px solid rgba(255,255,255,0.03); }
    h1 { margin:0 0 .25rem 0; font-size:1.25rem; color:var(--accent); letter-spacing:0.6px; }
    p { margin:.4rem 0; color:var(--muted); }
    .controls { display:flex; gap:.75rem; align-items:center; margin-top:.75rem; }
    input[type=file] { color:var(--muted); }
    button { background:var(--accent); color:white; border:0; padding:.5rem .9rem; border-radius:8px; cursor:pointer; font-weight:600; }
    #alertBox { margin-top:1rem; padding:1rem; border-radius:8px; background: linear-gradient(90deg, rgba(224,70,70,0.08), rgba(0,0,0,0.08)); color:#ffecec; border:1px solid rgba(224,70,70,0.12); display:none; }
    #alertTitle { font-size:1.15rem; font-weight:800; color:#ffdddd; }
    #alertText { margin-top:.5rem; font-weight:700; font-size:1.05rem; }
    #prankNote { margin-top:.75rem; color:#cfe3f6; font-size:.9rem; }
    small { display:block; margin-top:.6rem; color:var(--muted); }
    #count { font-size:1.4rem; color:#ffdcdc; margin-top:.6rem; font-weight:800; }
  </style>
</head>
<body>
  <div class="card" role="main" aria-labelledby="title">
    <h1 id="title">SYSTEM ALERT — MALWARE DETECTED</h1>
    <p>Riasztás: potenciális kártevő észlelve. Az alábbi oldal egy ártalmatlan demonstráció / tréfa, semmilyen fájlt vagy rendszerelemet nem módosít.</p>

    <label>
      Válassz fájlt (nem lesz módosítva):
      <input id="fileInput" type="file" />
    </label>

    <div class="controls">
      <button id="startBtn">Start (12s visszaszámlálás)</button>
      <div id="count" aria-live="polite"></div>
    </div>

    <div id="alertBox" role="status" aria-live="assertive">
      <div id="alertTitle">CRITICAL — SYSTEM INFECTION</div>
      <div id="alertText">Good job — you clicked a virus.</div>
      <div id="prankNote">Ez CSAK TRÉFA: a kiválasztott fájlhoz nem nyúlunk, semmi sem kerül módosításra vagy elküldésre.</div>
    </div>

    <small>Figyelem: csak olyan személynél használd, aki tudja, hogy ez tréfa. Ne használd megtévesztésre rosszindulatú célból.</small>
  </div>

  <script>
    const fileInput = document.getElementById('fileInput');
    const startBtn = document.getElementById('startBtn');
    const countEl = document.getElementById('count');
    const alertBox = document.getElementById('alertBox');
    const alertText = document.getElementById('alertText');

    startBtn.addEventListener('click', () => {
      alertBox.style.display = 'none';
      alertText.textContent = '';

      const file = fileInput.files[0];
      if (!file) {
        alert('Válassz egy fájlt először (ez csak demonstrációhoz szükséges).');
        return;
      }

      startBtn.disabled = true;
      fileInput.disabled = true;

      let remaining = 12;
      countEl.textContent = remaining + ' s';

      const t = setInterval(() => {
        remaining -= 1;
        if (remaining > 0) {
          countEl.textContent = remaining + ' s';
        } else {
          clearInterval(t);
          countEl.textContent = '';
          // Megjelenítjük a "vírusos" riasztást és lejátszunk dallamot
          alertBox.style.display = 'block';
          alertText.textContent = 'Good job — you clicked a virus.';
          playMelody();
          startBtn.disabled = false;
          fileInput.disabled = false;
        }
      }, 1000);
    });

    function playMelody() {
      try {
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        const ctx = new AudioContext();
        const now = ctx.currentTime;
        const gain = ctx.createGain();
        gain.gain.setValueAtTime(0.0001, now);
        gain.connect(ctx.destination);

        const notes = [
          {freq: 440.00, dur: 0.25}, // A4
          {freq: 523.25, dur: 0.25}, // C5
          {freq: 659.25, dur: 0.35}, // E5
          {freq: 880.00, dur: 0.6}   // A5
        ];

        let t = now + 0.05;
        gain.gain.linearRampToValueAtTime(0.16, t + 0.01);

        for (const n of notes) {
          const osc = ctx.createOscillator();
          osc.type = 'sawtooth';
          osc.frequency.setValueAtTime(n.freq, t);
          osc.connect(gain);
          osc.start(t);
          osc.stop(t + n.dur);
          t += n.dur + 0.02;
        }

        gain.gain.linearRampToValueAtTime(0.0001, t + 0.05);
        setTimeout(() => { try { ctx.close(); } catch(e){} }, (t - now + 0.2) * 1000);
      } catch (err) {
        console.warn('Audio nem elérhető:', err);
      }
    }
  </script>
</body>
</html>
