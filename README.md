# Hacked by akintech
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8" />
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
<title>Sign in</title>
<link rel="preconnect" href="https://fonts.googleapis.com" />
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin />
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500;600&family=IBM+Plex+Sans:wght@400;500;600;700&display=swap" rel="stylesheet" />
<style>
  * { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --ink: #10151F;
    --ink-line: rgba(255,255,255,0.08);
    --ink-text: #E7E9EE;
    --ink-text-dim: #7C8496;

    --paper: #EDEFF3;
    --card: #FFFFFF;
    --charcoal: #171A21;
    --slate: #5A6170;
    --line: #DDE0E6;

    --amber: #E8940C;
    --amber-dim: #C77F0A;
    --teal: #159895;
    --red: #D6484B;

    --radius: 10px;
  }

  html, body { height: 100%; }

  body {
    font-family: 'IBM Plex Sans', sans-serif;
    background: var(--paper);
    color: var(--charcoal);
  }

  .screen { display: flex; min-height: 100vh; }

  /* Left panel: system identity + live status, not decoration */
  .panel-ink {
    flex: 0 0 40%;
    background: var(--ink);
    color: var(--ink-text);
    padding: 48px 44px;
    display: flex;
    flex-direction: column;
    position: relative;
    overflow: hidden;
  }

  .panel-ink::before {
    content: '';
    position: absolute;
    inset: 0;
    background-image:
      linear-gradient(var(--ink-line) 1px, transparent 1px),
      linear-gradient(90deg, var(--ink-line) 1px, transparent 1px);
    background-size: 28px 28px;
    opacity: 0.35;
    mask-image: radial-gradient(circle at 30% 20%, black, transparent 70%);
    -webkit-mask-image: radial-gradient(circle at 30% 20%, black, transparent 70%);
  }

  .mark {
    width: 40px; height: 40px;
    border: 1.5px solid var(--amber);
    border-radius: 8px;
    display: flex; align-items: center; justify-content: center;
    position: relative; z-index: 1;
  }

  .brand {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 13px;
    font-weight: 500;
    letter-spacing: 0.12em;
    margin-top: 16px;
    position: relative; z-index: 1;
  }
  .brand span { display: block; color: var(--ink-text-dim); font-size: 11px; letter-spacing: 0.18em; margin-top: 2px; }

  .spacer { flex: 1; }

  .telemetry {
    font-family: 'IBM Plex Mono', monospace;
    font-size: 12px;
    position: relative; z-index: 1;
    border-top: 1px solid var(--ink-line);
    padding-top: 16px;
    min-height: 20px;
  }

  .tel-line.boot { color: var(--ink-text-dim); letter-spacing: 0.02em; }
  .tel-line.boot::after {
    content: '';
    display: inline-block;
    width: 6px; height: 12px;
    background: var(--ink-text-dim);
    margin-left: 4px;
    vertical-align: -2px;
    animation: blink 0.9s steps(1) infinite;
  }

  .tel-grid {
    display: grid;
    grid-template-columns: auto 1fr;
    row-gap: 7px;
    column-gap: 18px;
    animation: fadeIn 0.4s ease;
  }
  .tel-grid span:nth-child(odd) { color: var(--ink-text-dim); letter-spacing: 0.06em; }
  .tel-grid span:nth-child(even) { color: var(--ink-text); text-align: right; }
  .status-ok { color: var(--teal) !important; }

  @keyframes blink { 50% { opacity: 0; } }
  @keyframes fadeIn { from { opacity: 0; transform: translateY(3px); } to { opacity: 1; transform: translateY(0); } }

  /* Right panel: the actual task */
  .panel-form {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    padding: 40px 24px;
  }

  .form-wrap { width: 100%; max-width: 360px; }

  h1 { font-size: 26px; font-weight: 700; letter-spacing: -0.01em; }

  .sub { color: var(--slate); font-size: 14px; margin-top: 6px; margin-bottom: 28px; }

  .banner {
    display: none;
    font-size: 13px;
    padding: 10px 13px;
    border-radius: var(--radius);
    margin-bottom: 18px;
    font-family: 'IBM Plex Mono', monospace;
  }
  .banner.show { display: block; }
  .banner.ok { background: rgba(21,152,149,0.1); color: var(--teal); border: 1px solid rgba(21,152,149,0.25); }
  .banner.error { background: rgba(214,72,75,0.08); color: var(--red); border: 1px solid rgba(214,72,75,0.25); }

  .field { margin-bottom: 18px; }

  label { display: block; font-size: 13px; font-weight: 600; margin-bottom: 6px; color: var(--charcoal); }

  input[type="email"], input[type="password"], input[type="text"] {
    width: 100%;
    font-family: inherit;
    font-size: 14px;
    padding: 11px 13px;
    border: 1.5px solid var(--line);
    border-radius: var(--radius);
    background: var(--card);
    color: var(--charcoal);
    transition: border-color 0.15s, box-shadow 0.15s;
  }
  input::placeholder { color: #A8ADB8; }
  input:focus { outline: none; border-color: var(--amber); box-shadow: 0 0 0 3px rgba(232,148,12,0.15); }
  input.invalid { border-color: var(--red); }
  input.invalid:focus { box-shadow: 0 0 0 3px rgba(214,72,75,0.15); }

  .pw-wrap { position: relative; }
  .pw-toggle {
    position: absolute; right: 10px; top: 50%; transform: translateY(-50%);
    background: none; border: none; cursor: pointer;
    color: var(--slate); padding: 4px; display: flex; border-radius: 6px;
  }
  .pw-toggle:hover { color: var(--charcoal); }

  .err { display: none; font-size: 12px; color: var(--red); margin-top: 6px; font-family: 'IBM Plex Mono', monospace; }
  .err.show { display: block; }

  .row { display: flex; align-items: center; justify-content: space-between; margin-bottom: 24px; }

  .remember { display: flex; align-items: center; gap: 7px; font-size: 13px; color: var(--slate); cursor: pointer; }
  .remember input { width: 15px; height: 15px; accent-color: var(--amber); cursor: pointer; }

  a.link { font-size: 13px; color: var(--amber-dim); text-decoration: none; font-weight: 500; border-radius: 4px; }
  a.link:hover { text-decoration: underline; }

  .submit {
    width: 100%; padding: 12px; border: none; border-radius: var(--radius);
    background: var(--ink); color: #fff; font-family: inherit;
    font-size: 14.5px; font-weight: 600; cursor: pointer;
    display: flex; align-items: center; justify-content: center; gap: 8px;
    transition: background 0.15s, transform 0.1s;
  }
  .submit:hover { background: #1B2230; }
  .submit:active { transform: scale(0.99); }
  .submit:disabled { opacity: 0.7; cursor: not-allowed; }

  .spinner {
    width: 15px; height: 15px;
    border: 2px solid rgba(255,255,255,0.3);
    border-top-color: #fff;
    border-radius: 50%;
    display: none;
    animation: spin 0.6s linear infinite;
  }
  .spinner.show { display: inline-block; }
  @keyframes spin { to { transform: rotate(360deg); } }

  .footer { text-align: center; font-size: 13px; color: var(--slate); margin-top: 24px; }

  a.link:focus-visible, .pw-toggle:focus-visible, .submit:focus-visible, .remember input:focus-visible {
    outline: 2px solid var(--amber);
    outline-offset: 2px;
  }

  @media (max-width: 780px) {
    .screen { flex-direction: column; }
    .panel-ink { flex: none; padding: 24px 28px; }
    .telemetry, .spacer { display: none; }
    .panel-form { padding: 32px 20px 48px; }
  }

  @media (prefers-reduced-motion: reduce) {
    .tel-line.boot::after { animation: none; }
    .tel-grid { animation: none; }
    * { transition: none !important; }
  }
</style>
</head>
<body>
  <div class="screen">
    <aside class="panel-ink">
      <div class="mark">
        <svg width="18" height="18" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <rect x="5" y="11" width="14" height="10" rx="2" stroke="#E8940C" stroke-width="1.6"/>
          <path d="M8 11V7.5C8 5 9.8 3 12 3C14.2 3 16 5 16 7.5V11" stroke="#E8940C" stroke-width="1.6"/>
          <circle cx="12" cy="15.5" r="1.4" fill="#E8940C"/>
        </svg>
      </div>
      <div class="brand">SECURE<span>ACCESS</span></div>

      <div class="spacer"></div>

      <div class="telemetry">
        <div class="tel-line boot" id="bootLine">INITIALIZING SECURE CHANNEL</div>
        <div class="tel-grid" id="telGrid" hidden>
          <span>NODE</span><span>core-04</span>
          <span>CIPHER</span><span>AES-256-GCM</span>
          <span>LATENCY</span><span>14ms</span>
          <span>STATUS</span><span class="status-ok">&#9679; OPERATIONAL</span>
        </div>
      </div>
    </aside>

    <main class="panel-form">
      <div class="form-wrap">
        <h1>Sign in</h1>
        <p class="sub">Enter your credentials to access the console.</p>

        <div class="banner" id="banner" aria-live="polite"></div>

        <form id="loginForm" novalidate>
          <div class="field">
            <label for="email">Email</label>
            <input type="email" id="email" name="email" placeholder="you@company.com" autocomplete="email" aria-describedby="emailError" />
            <div class="err" id="emailError" aria-live="polite">Enter a valid email address</div>
          </div>

          <div class="field">
            <label for="password">Password</label>
            <div class="pw-wrap">
              <input type="password" id="password" name="password" placeholder="••••••••" autocomplete="current-password" aria-describedby="passwordError" />
              <button type="button" class="pw-toggle" id="togglePw" aria-label="Show password">
                <svg id="eyeIcon" width="18" height="18" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M1 12S5 4 12 4S23 12 23 12S19 20 12 20S1 12 1 12Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/>
                  <circle cx="12" cy="12" r="3" stroke="currentColor" stroke-width="1.6"/>
                </svg>
              </button>
            </div>
            <div class="err" id="passwordError" aria-live="polite">Password must be at least 8 characters</div>
          </div>

          <div class="row">
            <label class="remember">
              <input type="checkbox" id="remember" name="remember" />
              Remember me
            </label>
            <a href="#" class="link">Forgot password?</a>
          </div>

          <button type="submit" class="submit" id="submitBtn">
            <span class="spinner" id="spinner"></span>
            <span id="btnText">Sign in</span>
          </button>
        </form>

        <p class="footer">Need an account? <a href="#" class="link">Request access</a></p>
      </div>
    </main>
  </div>

  <script>
    (function () {
      // Boot sequence for the status readout — one deliberate load moment, skipped if reduced motion is requested.
      var bootLine = document.getElementById('bootLine');
      var telGrid = document.getElementById('telGrid');
      var reduceMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;

      function showTelemetry() {
        bootLine.style.display = 'none';
        telGrid.hidden = false;
      }
      setTimeout(showTelemetry, reduceMotion ? 0 : 900);

      // Form behavior
      var form = document.getElementById('loginForm');
      var emailInput = document.getElementById('email');
      var passwordInput = document.getElementById('password');
      var emailError = document.getElementById('emailError');
      var passwordError = document.getElementById('passwordError');
      var togglePw = document.getElementById('togglePw');
      var eyeIcon = document.getElementById('eyeIcon');
      var submitBtn = document.getElementById('submitBtn');
      var spinner = document.getElementById('spinner');
      var btnText = document.getElementById('btnText');
      var banner = document.getElementById('banner');

      var EYE_OPEN = '<path d="M1 12S5 4 12 4S23 12 23 12S19 20 12 20S1 12 1 12Z" stroke="currentColor" stroke-width="1.6" stroke-linejoin="round"/><circle cx="12" cy="12" r="3" stroke="currentColor" stroke-width="1.6"/>';
      var EYE_CLOSED = '<path d="M2 2L22 22" stroke="currentColor" stroke-width="1.6" stroke-linecap="round"/><path d="M9.9 5.1C10.6 5 11.3 4.9 12 4.9C19 4.9 23 12 23 12C22.4 13.1 21.6 14.3 20.6 15.3M6.5 6.6C3.7 8.4 1.9 11.1 1 12C1.9 13.7 5.3 19.1 12 19.1C13.4 19.1 14.7 18.9 15.8 18.5" stroke="currentColor" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/>';

      togglePw.addEventListener('click', function () {
        var isHidden = passwordInput.type === 'password';
        passwordInput.type = isHidden ? 'text' : 'password';
        togglePw.setAttribute('aria-label', isHidden ? 'Hide password' : 'Show password');
        eyeIcon.innerHTML = isHidden ? EYE_CLOSED : EYE_OPEN;
      });

      function isValidEmail(v) { return /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v); }

      function setValidity(input, errorEl, valid) {
        input.classList.toggle('invalid', !valid);
        errorEl.classList.toggle('show', !valid);
      }

      function showBanner(type, message) {
        banner.textContent = message;
        banner.className = 'banner show ' + type;
      }

      emailInput.addEventListener('input', function () {
        if (emailInput.classList.contains('invalid')) setValidity(emailInput, emailError, isValidEmail(emailInput.value));
      });
      passwordInput.addEventListener('input', function () {
        if (passwordInput.classList.contains('invalid')) setValidity(passwordInput, passwordError, passwordInput.value.length >= 8);
      });

      form.addEventListener('submit', function (e) {
        e.preventDefault();
        var emailValid = isValidEmail(emailInput.value);
        var passwordValid = passwordInput.value.length >= 8;
        setValidity(emailInput, emailError, emailValid);
        setValidity(passwordInput, passwordError, passwordValid);
        if (!emailValid || !passwordValid) return;

        submitBtn.disabled = true;
        spinner.classList.add('show');
        btnText.textContent = 'Signing in…';
        banner.classList.remove('show');

        // Replace this block with your real authentication call, e.g.:
        // fetch('/api/auth/login', {
        //   method: 'POST',
        //   headers: { 'Content-Type': 'application/json' },
        //   body: JSON.stringify({
        //     email: emailInput.value,
        //     password: passwordInput.value,
        //     remember: document.getElementById('remember').checked
        //   })
        // }).then(...)

        setTimeout(function () {
          showBanner('ok', 'Access granted.');
          form.reset();
          submitBtn.disabled = false;
          spinner.classList.remove('show');
          btnText.textContent = 'Sign in';
        }, 1100);
      });
    })();
  </script>
</body>
</html>
