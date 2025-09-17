<!doctype html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Secret Unlock</title>
<style>
:root{--bg:#0f1724;--card:#0b1220;--accent:#7dd3fc;--muted:#9aa7b2}
body{font-family:system-ui,-apple-system,Segoe UI,Roboto,"Helvetica Neue",Arial;background:linear-gradient(180deg,var(--bg),#071028);color:#fff;min-height:100vh;display:grid;place-items:center;padding:2rem}
.card{background:linear-gradient(180deg,rgba(255,255,255,0.02),rgba(255,255,255,0.01));padding:28px;border-radius:12px;max-width:540px;width:100%;box-shadow:0 6px 24px rgba(2,6,23,0.6)}
h1{margin:0 0 10px;font-size:20px}
.hint{color:var(--muted);font-size:13px;margin-bottom:16px}
input[type="text"]{width:100%;padding:12px 14px;border-radius:8px;border:1px solid rgba(255,255,255,0.06);background:transparent;color:inherit;font-size:15px;outline:none}
button{margin-top:12px;padding:10px 14px;border-radius:8px;border:0;background:var(--accent);color:#042029;font-weight:600;cursor:pointer}
.secret{margin-top:18px;padding:14px;background:#021424;border-radius:8px;border:1px solid rgba(255,255,255,0.03);display:none}
.error{color:#ff9b9b;margin-top:10px;font-weight:600}
.small{font-size:13px;color:var(--muted)}
</style>
</head>
<body>
<div class="card" role="main" aria-labelledby="t">
<h1 id="t">Enter the code</h1>
<div class="hint">Type the secret code your girlfriend knows and press <strong>Unlock</strong>.</div>

<label for="code" class="small">Code</label>
<input id="code" type="text" inputmode="text" autocomplete="off" placeholder="enter code..." />
<button id="btn">Unlock</button>

<div id="msg" class="error" role="alert" aria-live="polite" style="display:none"></div>

<div id="secret" class="secret" aria-hidden="true">
<h2>Message</h2>
<div id="secretText">You unlocked the message 🎉</div>
</div>

<div class="small" style="margin-top:12px">This version is client-only. Codes are stored in the page (not secure).</div>
</div>

<script>
/*
Simple mapping. WARNING: Visible in page source -> not secret-proof.
Format: codes = { "CODE": "Text shown when code used" }
*/
const CODES = {
"rose123": "Hey love — surprise picnic this Sunday at 2pm. ❤️",
"bluebird": "You make my days brighter. Meet me at the café at 18:00.",
"sunny2025": "Secret: You get an extra dessert tonight! 🍰"
};

const btn = document.getElementById('btn');
const input = document.getElementById('code');
const secretEl = document.getElementById('secret');
const secretText = document.getElementById('secretText');
const msg = document.getElementById('msg');

function showError(text){
msg.style.display = "block";
msg.textContent = text;
secretEl.style.display = "none";
}

function showSecret(text){
msg.style.display = "none";
secretText.textContent = text;
secretEl.style.display = "block";
secretEl.setAttribute('aria-hidden','false');
}

// basic hit enter support
input.addEventListener('keydown', (e) => { if(e.key === 'Enter') btn.click(); });

btn.addEventListener('click', () => {
const code = input.value.trim();
if(!code){ showError("Please type the code."); return; }

// case-insensitive match:
const matchKey = Object.keys(CODES).find(k => k.toLowerCase() === code.toLowerCase());
if(matchKey){
showSecret(CODES[matchKey]);
} else {
showError("Code not recognized. Try again.");
}
});
</script>
</body>
</html>
