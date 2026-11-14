# nasa-voyager-opportunity-reflector
THIS IS FOR MY NASA PROJECT
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no" />
  <title>Deep Space Signals | NASA Space Apps Bahrain</title>
  <style>
    * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', system-ui, sans-serif; }
    body { background: #050811; color: #f8fafc; text-align: center; padding: 12px; }
    header { margin: 8px 0 14px; }
    header h1 { color: #38bdf8; font-size: 1.8rem; letter-spacing: 1.5px; }
    header p { color: #94a3b8; font-size: 0.9rem; margin-top: 4px; }

    .game-wrapper { max-width: 840px; margin: 0 auto 24px; background: #0f172a; padding: 14px; border-radius: 16px; border: 1px solid #1e293b; position: relative; }
    .top-bar { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 8px; margin-bottom: 10px; }
    .tabs { display: flex; gap: 6px; flex-wrap: wrap; }
    button { background: #1e293b; color: #e2e8f0; border: 1px solid #38bdf8; padding: 8px 12px; border-radius: 8px; font-weight: 700; cursor: pointer; font-size: 0.85rem; transition: 0.15s; }
    button:hover, button.active { background: #38bdf8; color: #050811; }

    .telemetry { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; background: #020617; padding: 8px; border-radius: 10px; border: 1px solid #1e293b; margin-bottom: 10px; font-size: 0.82rem; }
    .telemetry div span { display: block; color: #38bdf8; font-weight: 800; font-size: 0.95rem; margin-top: 2px; }

    .canvas-container { position: relative; width: 100%; max-width: 800px; margin: 0 auto; }
    canvas { background: #02040a; border: 2px solid #38bdf8; border-radius: 10px; width: 100%; height: auto; display: block; touch-action: none; }

    /* Interactive Pop-up Overlay inside the Game Screen */
    #overlay { position: absolute; inset: 0; background: rgba(5, 8, 17, 0.92); border-radius: 10px; display: flex; flex-direction: column; justify-content: center; align-items: center; padding: 20px; z-index: 10; }
    #overlay.hidden { display: none; }
    #overlay h2 { color: #38bdf8; font-size: 1.4rem; margin-bottom: 10px; }
    #overlay p { color: #cbd5e1; max-width: 560px; line-height: 1.5; font-size: 0.95rem; margin-bottom: 14px; }
    .fact-badge { background: #0c4a6e; color: #7dd3fc; padding: 6px 12px; border-radius: 20px; font-size: 0.8rem; font-weight: bold; margin-bottom: 10px; }
    .quiz-options { display: flex; flex-direction: column; gap: 8px; width: 100%; max-width: 460px; margin-bottom: 12px; }
    .quiz-options button { width: 100%; text-align: left; padding: 10px 14px; font-size: 0.9rem; }

    .touch-controls { display: flex; justify-content: center; gap: 12px; margin-top: 12px; }
    .touch-controls button { padding: 14px 22px; font-size: 1rem; flex: 1; max-width: 180px; }
    .touch-controls button:active { background: #38bdf8; color: #050811; }

    .cards { display: grid; grid-template-columns: repeat(auto-fit, minmax(250px, 1fr)); gap: 14px; max-width: 840px; margin: 0 auto 24px; text-align: left; }
    .card { background: #0f172a; padding: 16px; border-radius: 12px; border-top: 4px solid #38bdf8; }
    .card h3 { color: #38bdf8; font-size: 1.05rem; margin-bottom: 6px; }
    .card p { color: #cbd5e1; font-size: 0.88rem; line-height: 1.45; }
  </style>
</head>
<body>

  <header>
    <h1>🚀 DEEP SPACE SIGNALS</h1>
    <p>Interactive STEM Physics & NASA Telemetry Simulator — NASA Space Apps Bahrain</p>
  </header>

  <div class="game-wrapper">
    <div class="top-bar">
      <div class="tabs">
        <button id="btn1" class="active" onclick="selectMission(1)">1. Moon Reflectors</button>
        <button id="btn2" onclick="selectMission(2)">2. Opportunity Rover</button>
        <button id="btn3" onclick="selectMission(3)">3. Voyager 1</button>
      </div>
      <button id="soundBtn" onclick="toggleSound()">🔊 Sound: ON</button>
    </div>

    <div class="telemetry">
      <div>DISTANCE FROM EARTH<span id="telDist">384,400 km</span></div>
      <div>LIGHT-SPEED DELAY<span id="telDelay">2.56 sec (Round-Trip)</span></div>
      <div>MISSION SCORE<span id="telScore">0 PTS</span></div>
    </div>

    <div class="canvas-container">
      <canvas id="game" width="800" height="420"></canvas>

      <!-- Interactive Mission Briefing & STEM Quiz Overlay -->
      <div id="overlay">
        <div class="fact-badge" id="ovBadge">MISSION BRIEFING</div>
        <h2 id="ovTitle">Mission 1: Lunar Laser Ranging</h2>
        <p id="ovText">Apollo astronauts left special retroreflector mirrors on the Moon. Fire laser pulses at the moving reflector to measure the exact Earth-Moon distance using the speed of light (299,792 km/s)!</p>
        <div class="quiz-options" id="quizBox"></div>
        <button id="ovBtn" onclick="resumeGame()">🚀 LAUNCH MISSION</button>
      </div>
    </div>

    <div class="touch-controls">
      <button onmousedown="hold('left',true)" onmouseup="hold('left',false)" ontouchstart="hold('left',true)" ontouchend="hold('left',false)">◀ LEFT</button>
      <button onclick="actionBtn()">⚡ FIRE / CALIBRATE</button>
      <button onmousedown="hold('right',true)" onmouseup="hold('right',false)" ontouchstart="hold('right',true)" ontouchend="hold('right',false)">RIGHT ▶</button>
    </div>
  </div>

  <section class="cards">
    <div class="card">
      <h3>📐 Formula: Distance = Speed × Time</h3>
      <p>Because light travels at a constant <strong>299,792 km/s</strong>, NASA scientists calculate exact planetary distances by timing how long a radio or laser pulse takes to travel across space.</p>
    </div>
    <div class="card">
      <h3>⏱️ Why Mars Rovers Can't Be Joystick-Driven</h3>
      <p>Radio signals take between <strong>4 and 24 minutes</strong> one-way to reach Mars. Engineers must send command sequences ahead of time and wait for telemetry confirmation.</p>
    </div>
    <div class="card">
      <h3>📡 NASA's Deep Space Network (DSN)</h3>
      <p>Three giant antenna complexes in California, Spain, and Australia communicate with distant probes like Voyager 1, whose signal is 20 billion times weaker than a digital watch battery.</p>
    </div>
  </section>

  <script>
    const canvas = document.getElementById('game');
    const ctx = canvas.getContext('2d');
    let level = 1, score = 0, paused = true, soundOn = true, milestoneHit = false;
    const keys = { left: false, right: false };

    // Simple Web Audio API synthesizer for arcade sound effects
    const audioCtx = new (window.AudioContext || window.webkitAudioContext)();
    function playTone(freq, type, duration) {
      if (!soundOn) return;
      if (audioCtx.state === 'suspended') audioCtx.resume();
      const osc = audioCtx.createOscillator();
      const gain = audioCtx.createGain();
      osc.type = type;
      osc.frequency.setValueAtTime(freq, audioCtx.currentTime);
      gain.gain.setValueAtTime(0.12, audioCtx.currentTime);
      gain.gain.exponentialRampToValueAtTime(0.001, audioCtx.currentTime + duration);
      osc.connect(gain);
      gain.connect(audioCtx.destination);
      osc.start();
      osc.stop(audioCtx.currentTime + duration);
    }

    function toggleSound() {
      soundOn = !soundOn;
      document.getElementById('soundBtn').textContent = soundOn ? '🔊 Sound: ON' : '🔇 Sound: OFF';
    }

    // Background stars & particles
    const stars = Array.from({ length: 75 }, () => ({
      x: Math.random() * 800, y: Math.random() * 420, r: Math.random() * 1.8 + 0.5
    }));
    let particles = [];
    function spawnParticles(x, y, color) {
      for (let i = 0; i < 14; i++) {
        particles.push({
          x, y,
          vx: (Math.random() - 0.5) * 6,
          vy: (Math.random() - 0.5) * 6,
          life: 25, color
        });
      }
    }

    // Mission Data & Interactive STEM Quizzes
    const missionInfo = {
      1: {
        dist: "384,400 km", delay: "2.56 sec (Round-Trip)",
        title: "Mission 1: Lunar Laser Ranging",
        text: "Apollo 11, 14, and 15 left corner-cube mirror arrays on the Moon. Aim your observatory laser at the moving reflector and press FIRE to measure the bounce-back time!",
        quizQ: "STEM CHECK: What key discovery did Lunar Laser Ranging prove about the Moon?",
        options: [
          { text: "A) The Moon is drifting away from Earth by ~3.8 cm per year", correct: true },
          { text: "B) The Moon is made completely of hollow metal", correct: false },
          { text: "C) Lasers travel faster than radio waves in space", correct: false }
        ]
      },
      2: {
        dist: "225 Million km (Avg)", delay: "12.5 mins (Simulated 0.65s)",
        title: "Mission 2: Opportunity Mars Rover",
        text: "Opportunity explored Mars for nearly 15 years! Because radio signals take minutes to cross space, your LEFT/RIGHT controls have a realistic signal delay. Plan your moves early to dodge boulders!",
        quizQ: "STEM CHECK: Why did Opportunity's mission finally end in 2018?",
        options: [
          { text: "A) It ran out of rocket fuel", correct: false },
          { text: "B) A massive global dust storm blocked sunlight from its solar panels", correct: true },
          { text: "C) It fell into a deep ocean on Mars", correct: false }
        ]
      },
      3: {
        dist: "24.9 Billion km", delay: "23+ Hours (One-Way)",
        title: "Mission 3: Voyager 1 Interstellar Link",
        text: "Launched in 1977, Voyager 1 is the farthest human-made object in space! Use LEFT and RIGHT to keep Voyager's High-Gain Antenna locked onto Earth's Deep Space Network beam.",
        quizQ: "STEM CHECK: What powers Voyager 1 so far from the Sun where solar panels don't work?",
        options: [
          { text: "A) Giant rechargeable lithium batteries", correct: false },
          { text: "B) Radioisotope Thermoelectric Generators (RTGs) using plutonium-238 heat", correct: true },
          { text: "C) Wind turbines catching solar wind", correct: false }
        ]
      }
    };

    // Game State Variables
    let moonX = 400, moonDir = 2.6, turretX = 400, laser = null, laserTimer = 0;
    let roverX = 400, rocks = [], cmdQueue = [];
    let dishAngle = 0, targetAngle = 0.35, signalLock = 0;

    function hold(dir, state) {
      if (paused) return;
      if (level === 2 && state) {
        cmdQueue.push({ dir, time: performance.now() + 650 });
        playTone(300, 'sine', 0.08);
      } else {
        keys[dir] = state;
      }
    }

    window.addEventListener('keydown', e => {
      if (e.key === 'ArrowLeft' || e.key === 'a') hold('left', true);
      if (e.key === 'ArrowRight' || e.key === 'd') hold('right', true);
      if (e.key === ' ' || e.key === 'Enter') actionBtn();
    });
    window.addEventListener('keyup', e => {
      if (e.key === 'ArrowLeft' || e.key === 'a') hold('left', false);
      if (e.key === 'ArrowRight' || e.key === 'd') hold('right', false);
    });

    function actionBtn() {
      if (paused) return;
      if (level === 1 && !laser) {
        laser = { x: turretX, y: 365, returning: false };
        laserTimer = 0;
        playTone(880, 'sawtooth', 0.15);
      } else if (level === 3) {
        targetAngle = (Math.random() - 0.5) * 1.5;
        playTone(520, 'triangle', 0.1);
      }
    }

    function selectMission(lvl) {
      level = lvl;
      score = 0;
      milestoneHit = false;
      rocks = []; cmdQueue = []; laser = null; signalLock = 0;
      [1, 2, 3].forEach(n => document.getElementById('btn' + n).classList.toggle('active', n === lvl));

      const info = missionInfo[lvl];
      document.getElementById('telDist').textContent = info.dist;
      document.getElementById('telDelay').textContent = info.delay;
      document.getElementById('telScore').textContent = "0 PTS";

      showBriefing(info);
    }

    function showBriefing(info) {
      paused = true;
      document.getElementById('overlay').classList.remove('hidden');
      document.getElementById('ovBadge').textContent = "MISSION BRIEFING";
      document.getElementById('ovTitle').textContent = info.title;
      document.getElementById('ovText').textContent = info.text;
      document.getElementById('quizBox').innerHTML = '';
      document.getElementById('ovBtn').style.display = 'inline-block';
      document.getElementById('ovBtn').textContent = '🚀 LAUNCH MISSION';
    }

    function triggerQuiz() {
      paused = true;
      milestoneHit = true;
      playTone(660, 'sine', 0.25);
      const info = missionInfo[level];
      document.getElementById('overlay').classList.remove('hidden');
      document.getElementById('ovBadge').textContent = "📡 TELEMETRY DATA UNLOCKED (+200 BONUS PTS)";
      document.getElementById('ovTitle').textContent = "Quick Student Science Check!";
      document.getElementById('ovText').textContent = info.quizQ;
      document.getElementById('ovBtn').style.display = 'none';

      const quizBox = document.getElementById('quizBox');
      quizBox.innerHTML = '';
      info.options.forEach(opt => {
        const b = document.createElement('button');
        b.textContent = opt.text;
        b.onclick = () => {
          if (opt.correct) {
            playTone(1040, 'sine', 0.3);
            addScore(200);
            document.getElementById('ovBadge').textContent = "✅ CORRECT! +200 BONUS POINTS";
            document.getElementById('ovText').textContent = "Awesome job! You mastered this NASA mission concept. Ready to continue?";
          } else {
            playTone(180, 'sawtooth', 0.3);
            document.getElementById('ovBadge').textContent = "💡 GOOD TRY! HERE IS THE SCIENCE:";
            const rightAns = info.options.find(o => o.correct).text;
            document.getElementById('ovText').textContent = "The correct answer is: " + rightAns;
          }
          quizBox.innerHTML = '';
          document.getElementById('ovBtn').style.display = 'inline-block';
          document.getElementById('ovBtn').textContent = '▶ CONTINUE MISSION';
        };
        quizBox.appendChild(b);
      });
    }

    function resumeGame() {
      paused = false;
      document.getElementById('overlay').classList.add('hidden');
    }

    function addScore(pts) {
      score += pts;
      document.getElementById('telScore').textContent = score + " PTS";
      if (score >= 200 && !milestoneHit) {
        triggerQuiz();
      }
    }

    function update() {
      ctx.clearRect(0, 0, 800, 420);
      ctx.fillStyle = '#ffffff';
      stars.forEach(s => { ctx.beginPath(); ctx.arc(s.x, s.y, s.r, 0, Math.PI * 2); ctx.fill(); });

      // Update & draw particles
      particles.forEach((p, idx) => {
        p.x += p.vx; p.y += p.vy; p.life--;
        ctx.fillStyle = p.color;
        ctx.fillRect(p.x, p.y, 4, 4);
        if (p.life <= 0) particles.splice(idx, 1);
      });

      if (!paused) {
        if (level === 1) {
          moonX += moonDir;
          if (moonX > 720 || moonX < 80) moonDir *= -1;
          if (keys.left) turretX = Math.max(40, turretX - 5.5);
          if (keys.right) turretX = Math.min(760, turretX + 5.5);
        }

        if (level === 2) {
          const now = performance.now();
          while (cmdQueue.length && cmdQueue[0].time <= now) {
            const cmd = cmdQueue.shift();
            roverX += cmd.dir === 'left' ? -50 : 50;
            roverX = Math.max(45, Math.min(755, roverX));
          }
          if (Math.random() < 0.028) rocks.push({ x: Math.random() * 720 + 40, y: -20, s: 2.6 + Math.random() * 1.8 });
        }

        if (level === 3) {
          targetAngle += Math.sin(performance.now() / 1300) * 0.007;
          if (keys.left) dishAngle -= 0.032;
          if (keys.right) dishAngle += 0.032;
        }
      }

      // Render Level 1
      if (level === 1) {
        ctx.fillStyle = '#94a3b8';
        ctx.beginPath(); ctx.arc(moonX, 70, 42, 0, Math.PI * 2); ctx.fill();
        ctx.fillStyle = '#64748b';
        ctx.beginPath(); ctx.arc(moonX - 14, 58, 9, 0, Math.PI * 2); ctx.arc(moonX + 16, 75, 6, 0, Math.PI * 2); ctx.fill();

        // Glowing Retroreflector
        ctx.fillStyle = '#38bdf8';
        ctx.fillRect(moonX - 20, 105, 40, 8);
        ctx.fillStyle = '#cbd5e1';
        ctx.font = '12px sans-serif';
        ctx.fillText('APOLLO RETROREFLECTOR', moonX - 72, 20);

        // Earth Observatory
        ctx.fillStyle = '#22c55e';
        ctx.fillRect(turretX - 24, 382, 48, 22);
        ctx.fillStyle = '#38bdf8';
        ctx.fillRect(turretX - 5, 366, 10, 16);

        if (laser && !paused) {
          laserTimer += 0.045;
          ctx.strokeStyle = laser.returning ? '#22c55e' : '#ef4444';
          ctx.lineWidth = 5;
          ctx.beginPath(); ctx.moveTo(laser.x, laser.y); ctx.lineTo(laser.x, laser.y + 28); ctx.stroke();

          ctx.fillStyle = '#f8fafc';
          ctx.font = '13px monospace';
          ctx.fillText(`PULSE TIME: ${laserTimer.toFixed(2)}s`, laser.x + 14, laser.y + 15);

          if (!laser.returning) {
            laser.y -= 11;
            if (laser.y <= 113) {
              if (Math.abs(laser.x - moonX) < 26) {
                laser.returning = true;
                spawnParticles(laser.x, 110, '#38bdf8');
                playTone(1200, 'sine', 0.12);
              } else {
                laser = null;
              }
            }
          } else {
            laser.y += 11;
            if (laser.y >= 366) {
              spawnParticles(laser.x, 370, '#22c55e');
              playTone(600, 'triangle', 0.2);
              addScore(100);
              laser = null;
            }
          }
        }
      }

      // Render Level 2
      if (level === 2) {
        ctx.fillStyle = '#7c2d12';
        ctx.fillRect(0, 362, 800, 58);

        // Opportunity Rover
        ctx.fillStyle = '#facc15';
        ctx.fillRect(roverX - 24, 330, 48, 22);
        ctx.fillStyle = '#0284c7';
        ctx.fillRect(roverX - 32, 323, 64, 7);
        ctx.fillStyle = '#e2e8f0';
        ctx.fillRect(roverX + 10, 310, 6, 14); // Camera mast
        ctx.beginPath(); ctx.arc(roverX - 15, 356, 7, 0, Math.PI * 2); ctx.arc(roverX + 15, 356, 7, 0, Math.PI * 2); ctx.fill();

        if (cmdQueue.length > 0) {
          ctx.fillStyle = '#facc15';
          ctx.font = 'bold 13px monospace';
          ctx.fillText('📡 SIGNAL IN TRANSIT (DELAY)...', roverX - 95, 298);
        }

        ctx.fillStyle = '#ea580c';
        rocks.forEach((r, i) => {
          if (!paused) r.y += r.s;
          ctx.beginPath(); ctx.arc(r.x, r.y, 16, 0, Math.PI * 2); ctx.fill();
          if (!paused) {
            if (Math.abs(r.x - roverX) < 32 && r.y > 312 && r.y < 362) {
              spawnParticles(roverX, 335, '#ef4444');
              playTone(140, 'sawtooth', 0.25);
              score = Math.max(0, score - 50);
              document.getElementById('telScore').textContent = score + " PTS";
              rocks.splice(i, 1);
            } else if (r.y > 372) {
              addScore(20);
              rocks.splice(i, 1);
            }
          }
        });
      }

      // Render Level 3
      if (level === 3) {
        const earthX = 400 + Math.sin(targetAngle) * 280;
        ctx.fillStyle = '#2563eb';
        ctx.beginPath(); ctx.arc(earthX, 60, 18, 0, Math.PI * 2); ctx.fill();
        ctx.fillStyle = '#94a3b8';
        ctx.font = 'bold 12px sans-serif';
        ctx.fillText('EARTH (DEEP SPACE NETWORK)', earthX - 85, 32);

        const aimX = 400 + Math.sin(dishAngle) * 280;
        const locked = Math.abs(aimX - earthX) < 35;

        ctx.strokeStyle = locked ? 'rgba(34, 197, 94, 0.7)' : 'rgba(239, 68, 68, 0.35)';
        ctx.lineWidth = locked ? 10 : 4;
        ctx.beginPath(); ctx.moveTo(400, 340); ctx.lineTo(aimX, 60); ctx.stroke();

        // Voyager 1 Gold Dish
        ctx.fillStyle = '#facc15';
        ctx.beginPath(); ctx.arc(400, 345, 25, Math.PI, 0); ctx.fill();
        ctx.fillStyle = '#cbd5e1';
        ctx.fillRect(392, 345, 16, 26);

        ctx.fillStyle = locked ? '#22c55e' : '#ef4444';
        ctx.font = 'bold 14px monospace';
        ctx.fillText(locked ? 'SIGNAL LOCKED: RECEIVING INTERSTELLAR DATA' : 'ALIGN DISH TO EARTH BEAM', 235, 405);

        if (locked && !paused) {
          signalLock++;
          if (signalLock % 18 === 0) {
            playTone(740, 'sine', 0.08);
            addScore(25);
          }
        }
      }

      requestAnimationFrame(update);
    }

    // Start with Mission 1 Briefing
    selectMission(1);
    update();
  </script>
</body>
</html>
