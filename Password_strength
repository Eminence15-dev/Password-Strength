<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Password Strength Checker</title>
  <style>
    @import url('https://fonts.googleapis.com/css2?family=DM+Mono:wght@300;400;500&family=Syne:wght@700;800&display=swap');
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body { background: #0a0a0f; color: #e2e2e8; font-family: 'DM Mono', 'Fira Code', monospace; padding: 2rem 1.5rem; min-height: 100vh; }
    ::selection { background: #a78bfa33; }
    .inner { max-width: 540px; margin: 0 auto; }
    .pw-input { width: 100%; background: #13131a; border: 1px solid #2a2a3a; color: #e2e2e8; padding: 14px 48px 14px 16px; font-family: 'DM Mono', monospace; font-size: 15px; border-radius: 8px; outline: none; transition: border-color 0.2s; letter-spacing: 0.04em; }
    .pw-input:focus { border-color: #4a4a6a; }
    .pw-input::placeholder { color: #333; }
    .toggle-vis { position: absolute; right: 14px; top: 50%; transform: translateY(-50%); background: none; border: none; color: #555; cursor: pointer; padding: 4px; display: flex; align-items: center; transition: color 0.2s; }
    .toggle-vis:hover { color: #aaa; }
    .bar { height: 3px; border-radius: 2px; flex: 1; transition: background 0.4s; }
    .check-item { display: flex; align-items: center; gap: 10px; padding: 5px 0; font-size: 12px; letter-spacing: 0.03em; }
    .dot { width: 6px; height: 6px; border-radius: 50%; flex-shrink: 0; transition: background 0.3s; }
    .mono-label { font-size: 10px; letter-spacing: 0.12em; text-transform: uppercase; color: #555; font-weight: 400; margin: 0 0 8px; }
    .safety-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
    @media (max-width: 480px) { .safety-grid { grid-template-columns: 1fr; } }
    .safety-card { background: #13131a; border: 1px solid #1e1e2e; border-radius: 10px; padding: 14px; transition: border-color 0.2s; }
    .safety-card:hover { border-color: #2e2e4e; }
    .metric-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 1.5rem; }
    .metric-card { background: #13131a; border: 1px solid #1e1e2e; border-radius: 8px; padding: 12px 14px; }
    .badge { font-size: 9px; font-weight: 500; letter-spacing: 0.08em; padding: 2px 6px; border-radius: 4px; }
  </style>
</head>
<body>
  <div class="inner">

    <div style="margin-bottom:2.5rem">
      <p style="font-family:'Syne',sans-serif;font-size:28px;font-weight:800;margin:0 0 4px;letter-spacing:-0.02em;color:#fff">Password checker</p>
      <p style="font-size:10px;color:#444;margin:0;letter-spacing:0.1em;text-transform:uppercase">Analysed locally — nothing is transmitted</p>
    </div>

    <div style="position:relative;margin-bottom:1.5rem">
      <input class="pw-input" type="password" id="pwInput" placeholder="enter a password…" autocomplete="off" spellcheck="false" />
      <button class="toggle-vis" id="toggleBtn">
        <svg id="eyeShow" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"><path d="M1 12s4-8 11-8 11 8 11 8-4 8-11 8-11-8-11-8z"/><circle cx="12" cy="12" r="3"/></svg>
        <svg id="eyeHide" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" style="display:none"><path d="M17.94 17.94A10.07 10.07 0 0 1 12 20c-7 0-11-8-11-8a18.45 18.45 0 0 1 5.06-5.94"/><path d="M9.9 4.24A9.12 9.12 0 0 1 12 4c7 0 11 8 11 8a18.5 18.5 0 0 1-2.16 3.19"/><line x1="1" y1="1" x2="23" y2="23"/></svg>
      </button>
    </div>

    <div id="results" style="display:none;margin-bottom:2rem">
      <div style="display:flex;align-items:center;justify-content:space-between;margin-bottom:10px">
        <span class="mono-label" style="margin:0">strength</span>
        <span id="scoreLabel" style="font-size:11px;font-weight:500;letter-spacing:0.08em;text-transform:uppercase"></span>
      </div>
      <div style="display:flex;gap:4px;margin-bottom:1.5rem">
        <div class="bar" id="b0"></div><div class="bar" id="b1"></div><div class="bar" id="b2"></div><div class="bar" id="b3"></div><div class="bar" id="b4"></div>
      </div>

      <div class="metric-grid">
        <div class="metric-card">
          <p class="mono-label">time to crack</p>
          <p id="crackVal" style="font-size:15px;font-weight:500;margin:0"></p>
        </div>
        <div class="metric-card">
          <p class="mono-label">entropy</p>
          <p id="entropyVal" style="font-size:15px;font-weight:500;margin:0"></p>
        </div>
      </div>

      <div id="tipBox" style="display:none;border-radius:8px;padding:10px 14px;margin-bottom:1.5rem;font-size:12px;color:#ccc;line-height:1.6"></div>

      <p class="mono-label">criteria</p>
      <div style="background:#13131a;border:1px solid #1e1e2e;border-radius:8px;padding:10px 14px" id="checkList"></div>
    </div>

    <div>
      <p class="mono-label">cyber safety essentials</p>
      <div class="safety-grid">
        <div class="safety-card">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:8px">
            <span class="badge" style="color:#a78bfa;background:#1a1030">2FA</span>
            <span style="font-size:12px;font-weight:500;color:#ccc">Two-factor auth</span>
          </div>
          <p style="font-size:11px;color:#555;line-height:1.6">Enable 2FA on all critical accounts — email, banking, socials.</p>
        </div>
        <div class="safety-card">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:8px">
            <span class="badge" style="color:#22d3ee;background:#07202e">PWD</span>
            <span style="font-size:12px;font-weight:500;color:#ccc">Password manager</span>
          </div>
          <p style="font-size:11px;color:#555;line-height:1.6">Use Bitwarden or 1Password. Never reuse passwords across sites.</p>
        </div>
        <div class="safety-card">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:8px">
            <span class="badge" style="color:#f5c518;background:#3a2e08">PHI</span>
            <span style="font-size:12px;font-weight:500;color:#ccc">Phishing awareness</span>
          </div>
          <p style="font-size:11px;color:#555;line-height:1.6">Verify sender addresses and URLs before entering any credentials.</p>
        </div>
        <div class="safety-card">
          <div style="display:flex;align-items:center;gap:8px;margin-bottom:8px">
            <span class="badge" style="color:#4ade80;background:#0d2e1a">UPD</span>
            <span style="font-size:12px;font-weight:500;color:#ccc">Keep software updated</span>
          </div>
          <p style="font-size:11px;color:#555;line-height:1.6">Updates patch vulnerabilities that attackers actively exploit.</p>
        </div>
      </div>
    </div>

    <p style="font-size:11px;color:#2a2a3a;margin-top:2rem;text-align:center;letter-spacing:0.04em">analysis runs in-browser · no data leaves your device</p>
  </div>

  <script>
    const COMMON = ['password','passw0rd','letmein','welcome','monkey','dragon','master','qwerty','admin','login','hello','shadow','sunshine','princess','football','baseball','superman','batman','iloveyou','trustno1'];
    const SEQS = ['abcdef','bcdefg','cdefgh','defghi','efghij','fghijk','ghijkl','hijklm','ijklmn','jklmno','klmnop','lmnopq','mnopqr','nopqrs','opqrst','pqrstu','qrstuv','rstuvw','stuvwx','tuvwxy','uvwxyz','012345','123456','234567','345678','456789','567890','qwerty','asdfgh','zxcvbn'];

    const SCORE_META = [
      {label:'Very weak',   color:'#ff4d4d', dim:'#3a1a1a'},
      {label:'Weak',        color:'#ff7a29', dim:'#3a2010'},
      {label:'Fair',        color:'#f5c518', dim:'#3a2e08'},
      {label:'Good',        color:'#4ade80', dim:'#0d2e1a'},
      {label:'Strong',      color:'#22d3ee', dim:'#072030'},
      {label:'Very strong', color:'#a78bfa', dim:'#1a1030'},
    ];

    const TIPS = [null,'Increase length to at least 12 characters.','Add uppercase, numbers, and symbols.','Remove dictionary words or common sequences.','A bit more length would make this excellent.','Strong — just avoid reusing it elsewhere.',null];

    const CHECKS_DEF = [
      {k:'len',   label:'At least 12 characters',    fn: p => p.length >= 12},
      {k:'upper', label:'Uppercase letters (A–Z)',    fn: p => /[A-Z]/.test(p)},
      {k:'lower', label:'Lowercase letters (a–z)',    fn: p => /[a-z]/.test(p)},
      {k:'num',   label:'Numbers (0–9)',              fn: p => /[0-9]/.test(p)},
      {k:'sym',   label:'Symbols (!@#$…)',            fn: p => /[^a-zA-Z0-9]/.test(p)},
      {k:'rep',   label:'No repeated characters',    fn: p => !(/(.)\1{2,}/.test(p))},
      {k:'seq',   label:'No common sequences',        fn: p => !SEQS.some(s => p.toLowerCase().includes(s))},
      {k:'word',  label:'No dictionary words',        fn: p => !COMMON.some(w => p.toLowerCase().includes(w))},
    ];

    function calcEntropy(pw) {
      let pool = 0;
      if (/[a-z]/.test(pw)) pool += 26;
      if (/[A-Z]/.test(pw)) pool += 26;
      if (/[0-9]/.test(pw)) pool += 10;
      if (/[^a-zA-Z0-9]/.test(pw)) pool += 32;
      return pw.length * Math.log2(pool || 1);
    }

    function crackTime(ent) {
      const s = Math.pow(2, ent) / 1e10;
      if (s < 1) return 'Instantly';
      if (s < 60) return Math.round(s) + ' seconds';
      if (s < 3600) return Math.round(s/60) + ' minutes';
      if (s < 86400) return Math.round(s/3600) + ' hours';
      if (s < 2592000) return Math.round(s/86400) + ' days';
      if (s < 31536000) return Math.round(s/2592000) + ' months';
      const y = s/31536000;
      if (y < 1000) return Math.round(y) + ' years';
      if (y < 1e6) return Math.round(y/1000) + 'k years';
      return 'Millions of years';
    }

    let showPw = false;
    document.getElementById('toggleBtn').onclick = () => {
      showPw = !showPw;
      document.getElementById('pwInput').type = showPw ? 'text' : 'password';
      document.getElementById('eyeShow').style.display = showPw ? 'none' : '';
      document.getElementById('eyeHide').style.display = showPw ? '' : 'none';
    };

    document.getElementById('pwInput').addEventListener('input', function() {
      const pw = this.value;
      const res = document.getElementById('results');
      if (!pw) { res.style.display = 'none'; return; }
      res.style.display = '';

      const checks = CHECKS_DEF.map(c => ({...c, pass: c.fn(pw)}));
      const passed = checks.filter(c => c.pass).length;
      const ent = calcEntropy(pw);
      let score;
      if (pw.length < 6) score = 0;
      else if (passed <= 3) score = 1;
      else if (passed <= 5) score = 2;
      else if (passed <= 6) score = 3;
      else if (ent < 60) score = 3;
      else if (ent < 80) score = 4;
      else score = 5;

      const meta = SCORE_META[score];
      document.getElementById('scoreLabel').textContent = meta.label;
      document.getElementById('scoreLabel').style.color = meta.color;
      document.getElementById('crackVal').textContent = crackTime(ent);
      document.getElementById('crackVal').style.color = meta.color;
      document.getElementById('entropyVal').textContent = Math.round(ent) + ' bits';
      document.getElementById('entropyVal').style.color = meta.color;

      for (let i = 0; i < 5; i++) {
        document.getElementById('b'+i).style.background = i < score ? meta.color : '#1e1e2e';
      }

      const tip = TIPS[score];
      const tipBox = document.getElementById('tipBox');
      if (tip) {
        tipBox.style.display = '';
        tipBox.style.background = meta.dim;
        tipBox.style.border = '1px solid ' + meta.color + '33';
        tipBox.textContent = tip;
      } else {
        tipBox.style.display = 'none';
      }

      document.getElementById('checkList').innerHTML = checks.map(c => `
        <div class="check-item">
          <div class="dot" style="background:${c.pass ? '#4ade80' : '#2a1010'};${c.pass ? '' : 'border:1px solid #ff4d4d44'}"></div>
          <span style="color:${c.pass ? '#777' : '#444'}">${c.label}</span>
        </div>
      `).join('');
    });
  </script>
</body>
</html>
