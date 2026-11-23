<script setup script>
import { ref } from "vue";

const copiedCmd = ref(null);
function copyCommand(cmd) {
  navigator.clipboard?.writeText(cmd);
  copiedCmd.value = cmd;
  setTimeout(() => {
    if (copiedCmd.value === cmd) copiedCmd.value = null;
  }, 1800);
}

const typewriterPhrases = [
  "Quantitative Researchers",
  "AI Coding Agents",
  "Prop-Firm Traders",
  "Systematic Algo Traders",
  "Autonomous Execution"
];

let i = 0;
let j = 0;
let currentWord = "";
let isDeleting = false;

function type() {
  currentWord = typewriterPhrases[i];
  const el = document.getElementById("typewriter");
  if (!el) return;

  if (isDeleting) {
    el.textContent = currentWord.substring(0, j - 1);
    j--;
    if (j === 0) {
      isDeleting = false;
      i = (i + 1) % typewriterPhrases.length;
    }
  } else {
    el.textContent = currentWord.substring(0, j + 1);
    j++;
    if (j === currentWord.length) {
      isDeleting = true;
      setTimeout(type, 1800);
      return;
    }
  }
  setTimeout(type, isDeleting ? 80 : 130);
}

const CONTRIBUTOR_REPOS = ["nujinAI/nujinSkills"];
const contributors = ref([]);

async function fetchContributors() {
  try {
    const byLogin = new Map();
    await Promise.all(
      CONTRIBUTOR_REPOS.map(async (repo) => {
        const res = await fetch(`https://api.github.com/repos/${repo}/contributors?per_page=100`);
        if (!res.ok) return;
        const list = await res.json();
        if (!Array.isArray(list)) return;
        for (const c of list) {
          if (!c?.login || c.type === "Bot") continue;
          const prev = byLogin.get(c.login);
          if (prev) {
            prev.contributions += c.contributions || 0;
          } else {
            byLogin.set(c.login, {
              login: c.login,
              avatar_url: c.avatar_url,
              html_url: c.html_url,
              contributions: c.contributions || 0
            });
          }
        }
      })
    );
    contributors.value = Array.from(byLogin.values()).sort(
      (a, b) => b.contributions - a.contributions
    );
  } catch {
    // Fallback to static invite if GitHub API is unreachable or rate-limited
  }
}

onMounted(() => {
  type();
  fetchContributors();
});
</script>

<template>
  <div class="bg-black/75 backdrop-blur-xl w-full min-h-screen text-gray-300 font-inter">
    <!-- Top Caution / Disclaimer Banner -->
    <div class="w-full bg-red-950/25 border-b border-red-900/40 px-6 py-3 sm:px-14 xl:px-36">
      <div class="flex items-start sm:items-center space-x-3 text-xs leading-relaxed text-gray-300">
        <span class="inline-flex items-center space-x-1.5 text-red-400 font-semibold uppercase tracking-wider flex-shrink-0">
          <svg class="w-4 h-4 fill-current inline" viewBox="0 0 20 20">
            <path fill-rule="evenodd" d="M8.257 3.099c.765-1.36 2.722-1.36 3.486 0l5.58 9.92c.75 1.334-.213 2.98-1.742 2.98H4.42c-1.53 0-2.493-1.646-1.743-2.98l5.58-9.92zM11 13a1 1 0 11-2 0 1 1 0 012 0zm-1-8a1 1 0 00-1 1v3a1 1 0 002 0V6a1 1 0 00-1-1z" clip-rule="evenodd"/>
          </svg>
          <span>Caution</span>
        </span>
        <span class="text-gray-400">
          This project is in an early stage and is provided for <strong class="text-gray-200">educational and research purposes only</strong>. Nothing herein constitutes financial or investment advice. Participating in financial markets carries substantial risk of loss—never trade or risk more capital than you are willing to lose.
        </span>
      </div>
    </div>

    <!-- Hero Section -->
    <section class="pt-8 sm:pt-12 pb-16">
      <div class="w-full px-6 sm:px-14 xl:px-36">
        <!-- Logo and Brand above Hero -->
        <div class="flex items-center space-x-3 mb-8" data-aos="fade-down">
          <img src="~/assets/logo.png" alt="NujinAI Logo" class="w-9 h-9 object-contain" />
          <span class="text-2xl font-bold tracking-tight text-white font-rubik bg-clip-text text-transparent bg-gradient-to-r from-teal-400 via-emerald-300 to-cyan-400 animate-gradient-text">
            NujinAI
          </span>
        </div>

        <div class="flex flex-col items-center lg:flex-row lg:items-center lg:justify-between gap-12">
          <div class="flex flex-col max-w-2xl text-left sm:items-center sm:text-center lg:text-left lg:items-start">
            <div class="inline-flex items-center gap-2 px-3 py-1.5 rounded-full border border-emerald-500/30 bg-emerald-950/40 text-emerald-400 text-xs font-mono mb-6" data-aos="fade-down">
              <span class="inline-block w-2 h-2 rounded-full bg-emerald-400 animate-pulse"></span>
              Open Source AI Quant Skill • 100% Free
            </div>

            <h1
              class="antialiased text-4xl sm:text-6xl md:text-7xl font-bold tracking-tight leading-tight text-white font-rubik"
              data-aos="fade-left"
            >
              Turn AI Agents into <br class="hidden sm:inline" />
              <span class="text-transparent bg-clip-text bg-gradient-to-r from-emerald-400 via-teal-300 to-cyan-400 animate-gradient-text">
                Quantitative Trading Engines
              </span>
            </h1>

            <p
              class="text-base sm:text-lg leading-relaxed text-gray-300 font-inter pt-6 max-w-xl"
              data-aos="fade-up"
            >
              Stop falling into retail traps. <strong>NujinAI</strong> uncovers market inefficiencies, develops trading strategies, rigorously battle-tests them, deploys your trading bots, and tracks real performance.
            </p>
            <p
              class="text-sm sm:text-base italic text-emerald-400 font-inter pt-3 max-w-xl"
              data-aos="fade-up"
            >
              A true trading framework from A to Z.
            </p>

            <div
              class="flex flex-col sm:flex-row items-stretch sm:items-center pt-8 gap-4"
              data-aos="fade-down"
            >
              <a
                href="https://www.linkedin.com/company/nujinai/"
                target="_blank"
                class="px-8 py-4 font-bold text-white bg-gradient-to-r from-fuchsia-500 via-pink-500 to-purple-600 animate-gradient-btn hover:opacity-95 rounded-xl whitespace-nowrap inline-flex items-center justify-center space-x-3 transition transform hover:-translate-y-0.5"
              >
                <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 20 20" fill="currentColor">
                  <path d="M17.04 17.043h-2.962v-4.64c0-1.107-.023-2.531-1.544-2.531c-1.544 0-1.78 1.204-1.78 2.449v4.722H7.793V7.5h2.844v1.3h.039c.397-.75 1.364-1.54 2.808-1.54c3.001 0 3.556 1.974 3.556 4.545zM4.447 6.194c-.954 0-1.72-.771-1.72-1.72s.767-1.72 1.72-1.72a1.72 1.72 0 0 1 0 3.44m1.484 10.85h-2.97V7.5h2.97zM18.522 0H1.476C.66 0 0 .645 0 1.44v17.12C0 19.355.66 20 1.476 20h17.042c.815 0 1.482-.644 1.482-1.44V1.44C20 .646 19.333 0 18.518 0z"/>
                </svg>
                <span>Follow on LinkedIn</span>
              </a>
              <a
                href="https://github.com/nujinAI/nujinSkills"
                target="_blank"
                class="px-6 py-4 font-semibold text-gray-300 hover:text-white border border-gray-800 hover:border-gray-700 bg-gray-900/60 rounded-xl whitespace-nowrap inline-flex items-center justify-center space-x-2 transition"
              >
                <span>Star on GitHub</span>
                <span class="text-xs px-2 py-0.5 rounded bg-emerald-500/20 text-emerald-400 font-mono">OSS</span>
              </a>
            </div>

            <!-- Get Started Link -->
            <div class="mt-6" data-aos="fade-up">
              <a
                href="#how-to-use"
                class="inline-flex items-center space-x-2 text-sm font-medium text-emerald-400 hover:text-emerald-300 border-b border-emerald-500/40 hover:border-emerald-300 pb-0.5 transition"
              >
                <span>Get started — see how it works</span>
                <span>&rarr;</span>
              </a>
            </div>
          </div>

          <!-- Hero Graphic: Terminal / Architecture Card -->
          <div class="w-full lg:max-w-xl" data-aos="fade-right">
            <div class="bg-gray-900 border border-gray-800 shadow-2xl rounded-2xl overflow-hidden">
              <div class="bg-gray-950 px-4 py-3 flex items-center justify-between border-b border-gray-800">
                <div class="flex items-center space-x-2">
                  <div class="w-3 h-3 rounded-full bg-red-500/80"></div>
                  <div class="w-3 h-3 rounded-full bg-yellow-500/80"></div>
                  <div class="w-3 h-3 rounded-full bg-green-500/80"></div>
                </div>
                <div class="text-xs font-mono text-gray-400">nujin-agent-microscope.sh</div>
                <div class="text-xs font-mono text-emerald-400">CPU: OK</div>
              </div>
              <div class="bg-black/90 p-6 font-mono text-xs sm:text-sm text-gray-300 space-y-3 leading-relaxed">
                <div class="text-gray-500">// 1. User Mandate Envelope</div>
                <div class="text-emerald-400">> "Build me a Gold prop-firm strategy with MDD &lt; 2.5%"</div>
                <div class="text-gray-500 pt-1">// 2. Mathematical Basis Grid (130 multi-scale coords)</div>
                <div class="text-cyan-300">✓ Decomposing raw OHLCV vectors [Fibonacci scales 2..55]</div>
                <div class="text-cyan-300">✓ Shallow Surrogate Tree & 5-Slice Sub-Era Invariance</div>
                <div class="text-gray-500 pt-1">// 3. Institutional 8-Gate Cynic Audit</div>
                <div class="grid grid-cols-2 gap-2 text-xs pt-1">
                  <div class="p-2 bg-gray-900/80 rounded border border-gray-800 flex justify-between">
                    <span>Gate 1: DSR</span>
                    <span class="text-emerald-400 font-bold">0.97 ≥ 0.95</span>
                  </div>
                  <div class="p-2 bg-gray-900/80 rounded border border-gray-800 flex justify-between">
                    <span>Gate 3: Monte Carlo</span>
                    <span class="text-emerald-400 font-bold">MDD 1.8%</span>
                  </div>
                  <div class="p-2 bg-gray-900/80 rounded border border-gray-800 flex justify-between">
                    <span>Gate 6: Friction</span>
                    <span class="text-emerald-400 font-bold">6.2% ≤ 10%</span>
                  </div>
                  <div class="p-2 bg-gray-900/80 rounded border border-gray-800 flex justify-between">
                    <span>Gate 8: Equity R²</span>
                    <span class="text-emerald-400 font-bold">0.89 ≥ 0.85</span>
                  </div>
                </div>
                <div class="text-emerald-400 font-semibold pt-2">
                  🚀 Strategy Approved: Ready for Live Supervised Paper Execution
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Mission & Philosophy Section -->
    <section id="about" class="py-20 border-t border-gray-900/80">
      <div class="px-6 sm:px-14 lg:px-24 xl:px-40">
        <div class="flex items-center justify-center">
          <h2
            class="antialiased text-3xl sm:text-5xl font-bold tracking-tight text-center text-white font-rubik"
            data-aos="fade-up"
          >
            Built for <span id="typewriter" class="text-transparent bg-clip-text bg-gradient-to-r from-teal-400 via-emerald-300 to-cyan-400 animate-gradient-text underline decoration-emerald-500/40"></span>
          </h2>
        </div>

        <div class="flex flex-col pt-16 md:flex-row items-center md:justify-between gap-12">
          <div class="flex flex-col w-full md:w-1/2" data-aos="fade-up">
            <div class="flex items-center">
              <img
                class="rounded-full w-16 h-16 object-cover border-2 border-emerald-500/30"
                src="~/assets/images/founder.jpg"
                alt="Christopher Müller"
              />
              <div class="pl-6">
                <div class="flex flex-wrap items-baseline text-base text-gray-200 sm:text-lg">
                  <a class="font-bold text-white hover:text-emerald-400 transition" href="https://chris.nujin.ai/" target="_blank">Christopher Müller</a>
                  <span class="pl-2 font-light text-emerald-400 text-sm">
                    Founder & Quant Architect
                  </span>
                </div>
                <div class="flex items-center space-x-3 pt-1 text-xs">
                  <a class="text-emerald-400 hover:text-emerald-300 font-mono transition" href="https://chris.nujin.ai/" target="_blank">
                    chris.nujin.ai &rarr;
                  </a>
                  <span class="text-gray-600">•</span>
                  <a class="text-gray-400 hover:text-emerald-400 transition" href="https://linkedin.com/in/christonomous" target="_blank">
                    LinkedIn
                  </a>
                </div>
              </div>
            </div>

            <div class="relative pt-8 text-base sm:text-lg text-gray-300 leading-relaxed space-y-5">
              <blockquote class="relative pl-5 py-3 pr-4 border-l-2 border-emerald-400 bg-emerald-950/20 rounded-r-xl italic text-gray-200">
                <span class="text-emerald-400/70 font-serif text-2xl leading-none mr-1">&ldquo;</span>Wealth should be a fundamental human right, not a privilege for the few. In an AI era and a K-shaped economy, finance should be accessible to everyone.<span class="text-emerald-400/70 font-serif text-2xl leading-none ml-1">&rdquo;</span>
              </blockquote>
              <p class="text-sm text-gray-400">
                Retail participants do not fail because financial markets are impossible—they fail because they lack institutional tooling, discipline, and adversarial validation. <strong>NujinAI</strong> replaces guesswork with a reproducible, standardized mathematical process.
              </p>
            </div>
          </div>

          <div class="w-full md:w-1/2" data-aos="zoom-in">
            <div class="p-8 rounded-2xl bg-gradient-to-br from-gray-900 to-black border border-gray-800 space-y-6">
              <div class="text-xs font-mono uppercase tracking-wider text-emerald-400">Core Principles</div>
              
              <div class="flex items-start space-x-4">
                <div class="flex-shrink-0 w-8 h-8 rounded-lg bg-emerald-950 border border-emerald-800 flex items-center justify-center text-emerald-400 font-bold">1</div>
                <div>
                  <h4 class="font-bold text-white text-base">Pure Mathematical Basis</h4>
                  <p class="text-xs sm:text-sm text-gray-400 pt-1">No subjective retail chart drawing or lagging indicators. We decompose raw tick/OHLCV data into fundamental physical vectors.</p>
                </div>
              </div>

              <div class="flex items-start space-x-4">
                <div class="flex-shrink-0 w-8 h-8 rounded-lg bg-emerald-950 border border-emerald-800 flex items-center justify-center text-emerald-400 font-bold">2</div>
                <div>
                  <h4 class="font-bold text-white text-base">Capital Protection</h4>
                  <p class="text-xs sm:text-sm text-gray-400 pt-1">Our system enforces strict discipline—preventing overtrading, sizing positions safely, and applying institutional-grade risk management.</p>
                </div>
              </div>

              <div class="flex items-start space-x-4">
                <div class="flex-shrink-0 w-8 h-8 rounded-lg bg-emerald-950 border border-emerald-800 flex items-center justify-center text-emerald-400 font-bold">3</div>
                <div>
                  <h4 class="font-bold text-white text-base">Adversarial Falsification</h4>
                  <p class="text-xs sm:text-sm text-gray-400 pt-1">Every discovered edge is tested against brutal Monte Carlo, regime shifts, and 8 institutional cynic gates before deployment.</p>
                </div>
              </div>
            </div>
          </div>
        </div>

        <!-- 3 Feature Pillars -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-8 pt-16">
          <div
            class="p-6 rounded-2xl bg-gray-900/60 border border-gray-800 hover:border-emerald-500/40 transition"
            data-aos="fade-up" data-aos-delay="100"
          >
            <div class="text-2xl mb-3">📐</div>
            <h3 class="text-xl font-bold text-white font-rubik">
              Describe Your Goal, Not Code
            </h3>
            <p class="pt-3 text-sm leading-relaxed text-gray-400">
              Tell NujinAI what you want in plain English—like scalping, swing trading, or passing prop-firm rules. The system handles the math and builds the strategy for you.
            </p>
          </div>

          <div
            class="p-6 rounded-2xl bg-gray-900/60 border border-gray-800 hover:border-emerald-500/40 transition"
            data-aos="fade-up" data-aos-delay="200"
          >
            <div class="text-2xl mb-3">🔬</div>
            <h3 class="text-xl font-bold text-white font-rubik">
              Built to Survive Any Market
            </h3>
            <p class="pt-3 text-sm leading-relaxed text-gray-400">
              Strategies aren't just tested on one lucky year. Every idea must prove it works across different market cycles—bull, bear, and sideways—so it doesn't break when conditions change.
            </p>
          </div>

          <div
            class="p-6 rounded-2xl bg-gray-900/60 border border-gray-800 hover:border-emerald-500/40 transition"
            data-aos="fade-up" data-aos-delay="300"
          >
            <div class="text-2xl mb-3">🛡️</div>
            <h3 class="text-xl font-bold text-white font-rubik">
              Real-World Costs Included
            </h3>
            <p class="pt-3 text-sm leading-relaxed text-gray-400">
              Most backtests lie by ignoring trading fees. NujinAI factors in real spreads, commissions, and slippage from day one—if a strategy can't profit after fees, it gets rejected.
            </p>
          </div>
        </div>
      </div>
    </section>

    <!-- 5-Phase Systematic Pipeline -->
    <section id="pipeline" class="py-24 bg-gradient-to-b from-transparent via-gray-950/60 to-transparent border-t border-gray-900/80">
      <div class="px-6 sm:px-14 lg:px-24 xl:px-40">
        <div class="text-center max-w-3xl mx-auto mb-16" data-aos="fade-up">
          <span class="text-xs font-mono uppercase tracking-widest text-emerald-400">System Architecture</span>
          <h2 class="text-3xl sm:text-5xl font-bold text-white font-rubik mt-2">
            The 5-Phase Systematic Alpha Pipeline
          </h2>
          <p class="text-gray-400 pt-4 text-sm sm:text-base">
            From natural language instruction to institutional-grade execution in 5 deterministic stages.
          </p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-5 gap-4">
          <div class="p-5 rounded-xl bg-gray-900/80 border border-gray-800 relative group hover:border-emerald-500/50 transition" data-aos="fade-up" data-aos-delay="100">
            <div class="text-xs font-mono text-emerald-400 font-bold mb-2">PHASE 01</div>
            <h4 class="font-bold text-white text-base mb-2">Set Your Rules</h4>
            <p class="text-xs text-gray-400 leading-relaxed">
              Define your risk limits, prop-firm rules, and trading style. NujinAI locks them in as hard guardrails.
            </p>
            <p class="text-[11px] font-medium text-emerald-400/90 pt-3">
              ✓ Never violates your risk limits
            </p>
          </div>

          <div class="p-5 rounded-xl bg-gray-900/80 border border-gray-800 relative group hover:border-emerald-500/50 transition" data-aos="fade-up" data-aos-delay="200">
            <div class="text-xs font-mono text-emerald-400 font-bold mb-2">PHASE 02</div>
            <h4 class="font-bold text-white text-base mb-2">Lock Unseen Data</h4>
            <p class="text-xs text-gray-400 leading-relaxed">
              Loads historical market data and seals off the final third as an unseen Blind Out-of-Sample test.
            </p>
            <p class="text-[11px] font-medium text-emerald-400/90 pt-3">
              ✓ Zero curve-fitting or hindsight bias
            </p>
          </div>

          <div class="p-5 rounded-xl bg-gray-900/80 border border-gray-800 relative group hover:border-emerald-500/50 transition" data-aos="fade-up" data-aos-delay="300">
            <div class="text-xs font-mono text-emerald-400 font-bold mb-2">PHASE 03</div>
            <h4 class="font-bold text-white text-base mb-2">Find Real Edges</h4>
            <p class="text-xs text-gray-400 leading-relaxed">
              Scans 130+ market features with machine learning to uncover repeatable inefficiencies across market eras.
            </p>
            <p class="text-[11px] font-medium text-emerald-400/90 pt-3">
              ✓ Proven to work in bull, bear &amp; sideways markets
            </p>
          </div>

          <div class="p-5 rounded-xl bg-gray-900/80 border border-gray-800 relative group hover:border-emerald-500/50 transition" data-aos="fade-up" data-aos-delay="400">
            <div class="text-xs font-mono text-emerald-400 font-bold mb-2">PHASE 04</div>
            <h4 class="font-bold text-white text-base mb-2">Build the Strategy</h4>
            <p class="text-xs text-gray-400 leading-relaxed">
              Turns validated patterns into clean, executable strategy code with minimal parameters and built-in sizing.
            </p>
            <p class="text-[11px] font-medium text-emerald-400/90 pt-3">
              ✓ Simple, transparent, and ready to run
            </p>
          </div>

          <div class="p-5 rounded-xl bg-gray-900/80 border border-gray-800 relative group hover:border-emerald-500/50 transition" data-aos="fade-up" data-aos-delay="500">
            <div class="text-xs font-mono text-emerald-400 font-bold mb-2">PHASE 05</div>
            <h4 class="font-bold text-white text-base mb-2">8-Gate Stress Test</h4>
            <p class="text-xs text-gray-400 leading-relaxed">
              Runs an institutional audit—Monte Carlo simulations, real trading fees, and drawdown checks—before launch.
            </p>
            <p class="text-[11px] font-medium text-emerald-400/90 pt-3">
              ✓ Only battle-tested bots go live
            </p>
          </div>
        </div>

        <div class="mt-8 text-center" data-aos="fade-up">
          <p class="inline-block px-5 py-2.5 rounded-full bg-emerald-950/50 border border-emerald-500/30 text-xs sm:text-sm text-emerald-300 font-inter">
            💡 <span class="font-semibold text-white">Risk-Free Paper Trading:</span> Let your bots run live in paper trade mode first—zero real money at risk until you feel completely comfortable.
          </p>
        </div>
      </div>
    </section>

    <!-- 8-Gate Cynic Audit Table -->
    <section id="gates" class="py-20 border-t border-gray-900/80">
      <div class="px-6 sm:px-14 lg:px-24 xl:px-40">
        <div class="text-center max-w-2xl mx-auto mb-14" data-aos="fade-up">
          <span class="text-xs font-mono uppercase tracking-widest text-emerald-400">Institutional Falsification</span>
          <h2 class="text-3xl sm:text-4xl font-bold text-white font-rubik mt-2">
            The 8 Institutional Cynic Stress Gates
          </h2>
          <p class="text-gray-400 pt-3 text-sm">
            Before any strategy sees live execution, it must survive the full adversarial falsification suite.
          </p>
        </div>

        <div class="overflow-x-auto rounded-2xl border border-gray-800 bg-gray-950/60 shadow-xl" data-aos="fade-up">
          <table class="w-full text-left text-xs sm:text-sm">
            <thead class="bg-gray-900/90 text-gray-300 font-mono text-xs uppercase tracking-wider border-b border-gray-800">
              <tr>
                <th class="p-4">Gate</th>
                <th class="p-4">Verification Check</th>
                <th class="p-4">Pass Condition</th>
                <th class="p-4 hidden md:table-cell">Institutional Purpose</th>
              </tr>
            </thead>
            <tbody class="divide-y divide-gray-800/80 font-mono text-xs">
              <tr class="hover:bg-gray-900/40 transition">
                <td class="p-4 font-bold text-emerald-400">Gate 1</td>
                <td class="p-4 text-white">Deflated Sharpe Ratio (DSR)</td>
                <td class="p-4 text-teal-300">DSR ≥ 0.95</td>
                <td class="p-4 text-gray-400 hidden md:table-cell font-sans">Penalizes Sharpe for selection bias and trial multiplicity (López de Prado).</td>
              </tr>
              <tr class="hover:bg-gray-900/40 transition">
                <td class="p-4 font-bold text-emerald-400">Gate 2</td>
                <td class="p-4 text-white">Parameter Stability Surface</td>
                <td class="p-4 text-teal-300">&lt; 30% drop over ±10% range</td>
                <td class="p-4 text-gray-400 hidden md:table-cell font-sans">Rejects overfitted "needle-in-a-haystack" fragile parameter peaks.</td>
              </tr>
              <tr class="hover:bg-gray-900/40 transition">
                <td class="p-4 font-bold text-emerald-400">Gate 3</td>
                <td class="p-4 text-white">Monte Carlo Ruin Stress</td>
                <td class="p-4 text-teal-300">MDD₉₉ ≤ 3.0%, Tail ≤ 2.5</td>
                <td class="p-4 text-gray-400 hidden md:table-cell font-sans">Falsifies sequence risk and fat-tail ruin probability across 1,000 iterations.</td>
              </tr>
              <tr class="hover:bg-gray-900/40 transition">
                <td class="p-4 font-bold text-emerald-400">Gate 4</td>
                <td class="p-4 text-white">Walk-Forward Combinatorial (CPCV)</td>
                <td class="p-4 text-teal-300">OOS Retention ≥ 50%</td>
                <td class="p-4 text-gray-400 hidden md:table-cell font-sans">Prevents regime overfitting across non-contiguous chronological blocks.</td>
              </tr>
              <tr class="hover:bg-gray-900/40 transition">
                <td class="p-4 font-bold text-emerald-400">Gate 5</td>
                <td class="p-4 text-white">Regime Shift Survival</td>
                <td class="p-4 text-teal-300">Profitable in Bull, Bear, Range</td>
                <td class="p-4 text-gray-400 hidden md:table-cell font-sans">Guarantees resilience across macro regime changes.</td>
              </tr>
              <tr class="hover:bg-gray-900/40 transition">
                <td class="p-4 font-bold text-emerald-400">Gate 6</td>
                <td class="p-4 text-white">Microstructure Friction Clearance</td>
                <td class="p-4 text-teal-300">Cost ≤ 10% of stop distance</td>
                <td class="p-4 text-gray-400 hidden md:table-cell font-sans">Guarantees edge is not eaten alive by real-world spread and slippage.</td>
              </tr>
              <tr class="hover:bg-gray-900/40 transition">
                <td class="p-4 font-bold text-emerald-400">Gate 7</td>
                <td class="p-4 text-white">Price Noise Jitter Resilience</td>
                <td class="p-4 text-teal-300">≥ 80% Sharpe retention</td>
                <td class="p-4 text-gray-400 hidden md:table-cell font-sans">Rejects strategies dependent on micro-tick sequence luck.</td>
              </tr>
              <tr class="hover:bg-gray-900/40 transition">
                <td class="p-4 font-bold text-emerald-400">Gate 8</td>
                <td class="p-4 text-white">Equity Linearity</td>
                <td class="p-4 text-teal-300">R² ≥ 0.85, K-Ratio ≥ 1.5</td>
                <td class="p-4 text-gray-400 hidden md:table-cell font-sans">Demands steady compounding growth over erratic outlier spikes.</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </section>

    <!-- Features & Execution Workflows -->
    <section id="features" class="py-24 border-t border-gray-900/80">
      <div class="px-6 sm:px-14 lg:px-24 xl:px-40">
        <div class="flex items-center justify-center">
          <div class="text-center" data-aos="fade-up">
            <span class="text-xs font-mono uppercase tracking-widest text-emerald-400">Universal Versatility</span>
            <h2 class="antialiased text-3xl sm:text-5xl font-bold tracking-tight text-white font-rubik mt-2">
              Tailored for Every Trading Horizon
            </h2>
            <p class="pt-3 text-sm text-gray-400 max-w-lg mx-auto">
              Tell your AI coding agent what you need in plain English—NujinAI handles the mathematical discovery and execution.
            </p>
          </div>
        </div>

        <div class="flex flex-col md:flex-row gap-12 pt-16">
          <div class="flex flex-col w-full md:w-1/2 space-y-6">
            <div
              class="p-6 rounded-2xl bg-gray-900/60 border border-gray-800 hover:border-emerald-500/40 transition"
              data-aos="fade-right" data-aos-delay="100"
            >
              <h3 class="text-lg font-bold text-white font-rubik flex items-center space-x-2">
                <span class="text-emerald-400 font-mono">01.</span>
                <span>Prop-Firm Day Trading (5m–15m)</span>
              </h3>
              <p class="pt-2 text-sm leading-relaxed text-gray-400">
                Engineered with strict intraday drawdown limits (&le; 2.0%) aligned with commercial prop-firm rulebooks.
              </p>
            </div>

            <div
              class="p-6 rounded-2xl bg-gray-900/60 border border-gray-800 hover:border-emerald-500/40 transition"
              data-aos="fade-right" data-aos-delay="200"
            >
              <h3 class="text-lg font-bold text-white font-rubik flex items-center space-x-2">
                <span class="text-emerald-400 font-mono">02.</span>
                <span>High-Fidelity Algo Scalping (1m–5m)</span>
              </h3>
              <p class="pt-2 text-sm leading-relaxed text-gray-400">
                Short holding horizons, strict friction clearance multiples, and tight drawdown containment.
              </p>
            </div>

            <div
              class="p-6 rounded-2xl bg-gray-900/60 border border-gray-800 hover:border-emerald-500/40 transition"
              data-aos="fade-right" data-aos-delay="300"
            >
              <h3 class="text-lg font-bold text-white font-rubik flex items-center space-x-2">
                <span class="text-emerald-400 font-mono">03.</span>
                <span>Macro Swing & Investment Plans (1h–1D)</span>
              </h3>
              <p class="pt-2 text-sm leading-relaxed text-gray-400">
                Multi-day holding horizons, structural volatility stops, and macro dispersion regime allocation.
              </p>
            </div>
          </div>

          <!-- Code Mockup -->
          <div class="w-full md:w-1/2" data-aos="zoom-in">
            <div class="bg-gray-900 border border-gray-800 shadow-2xl rounded-2xl overflow-hidden">
              <div class="bg-gray-950 px-4 py-3 flex items-center justify-between border-b border-gray-800">
                <div class="flex items-center space-x-2">
                  <div class="w-3 h-3 rounded-full bg-red-500/80"></div>
                  <div class="w-3 h-3 rounded-full bg-yellow-500/80"></div>
                  <div class="w-3 h-3 rounded-full bg-green-500/80"></div>
                </div>
                <div class="text-xs font-mono text-gray-400">nujinSkills/agent-workflow.md</div>
                <div class="text-xs font-mono text-emerald-400">v1.0</div>
              </div>
              <div class="p-6 bg-black font-mono text-xs sm:text-sm text-gray-300 leading-relaxed space-y-2">
                <div class="text-emerald-400 font-bold"># Natural Language Example:</div>
                <div class="text-gray-400 italic">"I want a day trading strategy on Gold (XAUUSD) for my $10,000 prop-firm account with a strict 3% max drawdown limit."</div>
                <div class="pt-4 text-emerald-400 font-bold"># Agent Execution Output:</div>
                <div class="text-cyan-300">✓ Anomaly Discovered: W=13 displacement vs relative volume impulse</div>
                <div class="text-cyan-300">✓ Sub-Era Invariance: 5/5 Chronological slices passed</div>
                <div class="text-cyan-300">✓ Cynic Audit: 8/8 Gates Passed (DSR: 0.98, R²: 0.88)</div>
                <div class="text-cyan-300">✓ Live Paper Bot Initialized with Telemetry API</div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- How to Use Section -->
    <section id="how-to-use" class="py-24 border-t border-gray-900/80 bg-gradient-to-b from-gray-950 via-black to-gray-950">
      <div class="px-6 sm:px-14 lg:px-24 xl:px-40">
        <div class="text-center max-w-3xl mx-auto mb-16" data-aos="fade-up">
          <span class="text-xs font-mono uppercase tracking-widest text-emerald-400">Zero Quant Knowledge Needed</span>
          <h2 class="text-3xl sm:text-5xl font-bold text-white font-rubik mt-2">
            How It Works in 3 Simple Steps
          </h2>
          <p class="text-gray-400 pt-4 text-base leading-relaxed">
            You don't need to write mathematical formulas. Your AI Agent knows the full framework and handles everything from discovery to deployment. You have two options to use NujinAI: install the skill into your existing agent, or use our dedicated NujinAgent.
          </p>
          <p class="mt-3 text-xs text-gray-400">
            🔄 <span class="text-gray-300 font-medium">Rolling updates multiple times a week.</span> Click <a href="https://github.com/nujinAI/nujinSkills" target="_blank" class="text-emerald-400 hover:underline">Star &amp; Watch on GitHub</a> to stay up to date.
          </p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-8" data-aos="fade-up">
          <!-- Step 1 -->
          <div class="p-8 rounded-2xl bg-gradient-to-b from-gray-900/80 to-black border border-gray-800 hover:border-emerald-500/40 transition flex flex-col justify-between group">
            <div>
              <div class="flex items-center justify-between mb-6">
                <span class="w-10 h-10 rounded-xl bg-emerald-950/80 border border-emerald-800/80 text-emerald-400 font-mono font-bold flex items-center justify-center text-sm">
                  01
                </span>
                <span class="text-2xl">📥</span>
              </div>
              <h3 class="text-xl font-bold text-white font-rubik mb-2">Install the Skill</h3>
              <p class="text-sm text-gray-400 leading-relaxed">
                Install in one command via the universal <span class="text-gray-300 font-medium">Agent Skills</span> standard:
              </p>

              <!-- Primary: npx skills add -->
              <div class="mt-4">
                <button
                  type="button"
                  @click="copyCommand('npx skills add nujinAI/nujinSkills')"
                  class="w-full p-3 bg-black/90 rounded-xl border border-emerald-500/40 hover:border-emerald-400 font-mono text-xs text-emerald-400 flex items-center justify-between gap-2 text-left transition cursor-pointer"
                >
                  <span class="overflow-x-auto">npx skills add nujinAI/nujinSkills</span>
                  <span class="text-[10px] shrink-0 px-1.5 py-0.5 rounded bg-emerald-500/10 text-emerald-300">
                    {{ copiedCmd === 'npx skills add nujinAI/nujinSkills' ? 'Copied!' : 'Copy' }}
                  </span>
                </button>
              </div>

              <!-- Secondary: Git Clone or ZIP -->
              <div class="mt-3">
                <div class="text-[10px] font-mono uppercase tracking-wider text-gray-500 mb-1.5">or clone / download manually</div>
                <button
                  type="button"
                  @click="copyCommand('git clone https://github.com/nujinAI/nujinSkills.git')"
                  class="w-full p-2.5 bg-black/90 rounded-xl border border-gray-800/90 hover:border-gray-600 font-mono text-[11px] text-gray-400 flex items-center justify-between gap-2 text-left transition cursor-pointer"
                >
                  <span class="overflow-x-auto">git clone https://github.com/nujinAI/nujinSkills.git</span>
                  <span class="text-[10px] shrink-0 px-1.5 py-0.5 rounded bg-gray-800 text-gray-300">
                    {{ copiedCmd === 'git clone https://github.com/nujinAI/nujinSkills.git' ? 'Copied!' : 'Copy' }}
                  </span>
                </button>
                <a
                  href="https://github.com/nujinAI/nujinSkills/archive/refs/heads/main.zip"
                  class="mt-2 inline-flex items-center space-x-1.5 text-[11px] text-gray-400 hover:text-emerald-400 transition"
                >
                  <span>No terminal? Download ZIP &amp; open the folder in your AI agent</span>
                </a>
              </div>
            </div>
            <div class="pt-6 text-xs text-gray-500 font-mono">
              Ready in 5 seconds
            </div>
          </div>

          <!-- Step 2 -->
          <div class="p-8 rounded-2xl bg-gradient-to-b from-gray-900/80 to-black border border-gray-800 hover:border-emerald-500/40 transition flex flex-col justify-between group">
            <div>
              <div class="flex items-center justify-between mb-6">
                <span class="w-10 h-10 rounded-xl bg-emerald-950/80 border border-emerald-800/80 text-emerald-400 font-mono font-bold flex items-center justify-center text-sm">
                  02
                </span>
                <span class="text-2xl">⚡</span>
              </div>
              <h3 class="text-xl font-bold text-white font-rubik mb-2">Open Your AI Agent</h3>
              <p class="text-sm text-gray-400 leading-relaxed">
                Ask your agent in plain English—it loads <span class="text-gray-300 font-mono">SKILL.md</span> and sets up its Python environment automatically on first run:
              </p>
              <div class="mt-4 flex flex-wrap gap-2 text-xs font-mono">
                <span class="px-2.5 py-1 rounded-lg bg-gray-900 border border-gray-800 text-gray-300">Antigravity</span>
                <span class="px-2.5 py-1 rounded-lg bg-gray-900 border border-gray-800 text-gray-300">Claude Code</span>
                <span class="px-2.5 py-1 rounded-lg bg-gray-900 border border-gray-800 text-gray-300">Cursor</span>
                <span class="px-2.5 py-1 rounded-lg bg-gray-900 border border-gray-800 text-gray-300">OpenClaw</span>
                <span class="px-2.5 py-1 rounded-lg bg-gray-900 border border-gray-800 text-gray-300">Hermes</span>
              </div>
            </div>
            <div class="pt-6 text-xs text-gray-500 font-mono">
              Reads SKILL.md automatically
            </div>
          </div>

          <!-- Step 3 -->
          <div class="p-8 rounded-2xl bg-gradient-to-b from-gray-900/80 to-black border border-gray-800 hover:border-emerald-500/40 transition flex flex-col justify-between group">
            <div>
              <div class="flex items-center justify-between mb-6">
                <span class="w-10 h-10 rounded-xl bg-emerald-950/80 border border-emerald-800/80 text-emerald-400 font-mono font-bold flex items-center justify-center text-sm">
                  03
                </span>
                <span class="text-2xl">💬</span>
              </div>
              <h3 class="text-xl font-bold text-white font-rubik mb-2">Speak Naturally</h3>
              <p class="text-sm text-gray-400 leading-relaxed">
                Just state your trading goals in plain English:
              </p>
              <div class="mt-4 p-3 bg-black/90 rounded-xl border border-gray-800/90 font-mono text-xs text-teal-300 italic">
                "Build me a prop-firm day trading strategy for Gold with MDD &lt; 2.5%"
              </div>
            </div>
            <div class="pt-6 text-xs text-gray-500 font-mono">
              Agent handles research & execution
            </div>
          </div>
        </div>

        <div class="my-10 border-t border-gray-800/80"></div>

        <!-- Option 2: Dedicated NujinAgent -->
        <div class="mt-8 p-6 sm:p-8 rounded-2xl bg-gradient-to-r from-gray-900/90 via-black to-gray-900/90 border border-emerald-500/30" data-aos="fade-up">
          <div class="mb-6">
            <div class="inline-flex items-center space-x-2 text-xs font-mono text-emerald-400 uppercase tracking-wider mb-2">
              <span>⚡</span>
              <span>Or use the dedicated agent</span>
            </div>
            <h3 class="text-xl font-bold text-white font-rubik mb-1.5">No AI Agent Installed? Use NujinAgent</h3>
            <p class="text-sm text-gray-400 leading-relaxed">
              Our dedicated AI agent ships with <span class="text-gray-200 font-medium">nujinSkills</span> preinstalled and includes a built-in WebUI.
            </p>
          </div>

          <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
            <!-- Step 1 -->
            <div class="p-4 rounded-xl bg-black/60 border border-gray-800">
              <div class="text-xs font-mono text-emerald-400 mb-1.5">01 · Install</div>
              <p class="text-xs text-gray-400 mb-3">Clone and install NujinAgent:</p>
              <button
                type="button"
                @click="copyCommand('git clone https://github.com/nujinAI/nujinAgent.git && cd nujinAgent && pip install -e .')"
                class="w-full p-2.5 bg-black/90 rounded-lg border border-emerald-500/40 hover:border-emerald-400 font-mono text-[11px] text-emerald-400 flex items-center justify-between gap-2 text-left transition cursor-pointer"
              >
                <span class="overflow-x-auto">git clone https://github.com/nujinAI/nujinAgent.git &amp;&amp; cd nujinAgent &amp;&amp; pip install -e .</span>
                <span class="text-[10px] shrink-0 px-1.5 py-0.5 rounded bg-emerald-500/10 text-emerald-300">
                  {{ copiedCmd === 'git clone https://github.com/nujinAI/nujinAgent.git && cd nujinAgent && pip install -e .' ? 'Copied!' : 'Copy' }}
                </span>
              </button>
            </div>

            <!-- Step 2 -->
            <div class="p-4 rounded-xl bg-black/60 border border-gray-800 space-y-2.5">
              <div class="text-xs font-mono text-emerald-400">02 · Connect Your AI</div>
              <div>
                <div class="text-[11px] text-gray-400 mb-1"><span class="text-gray-200 font-medium">WebUI</span> (start in terminal, then add key in Settings → Providers):</div>
                <button
                  type="button"
                  @click="copyCommand('nujinagent webui')"
                  class="w-full p-2 bg-black/90 rounded-lg border border-gray-800 hover:border-gray-600 font-mono text-[11px] text-gray-300 flex items-center justify-between gap-2 text-left transition cursor-pointer"
                >
                  <span>nujinagent webui</span>
                  <span class="text-[10px] shrink-0 px-1.5 py-0.5 rounded bg-gray-800 text-gray-300">
                    {{ copiedCmd === 'nujinagent webui' ? 'Copied!' : 'Copy' }}
                  </span>
                </button>
              </div>
              <div>
                <div class="text-[11px] text-gray-400 mb-1"><span class="text-gray-200 font-medium">CLI</span>:</div>
                <button
                  type="button"
                  @click="copyCommand('nujinagent onboard')"
                  class="w-full p-2 bg-black/90 rounded-lg border border-gray-800 hover:border-gray-600 font-mono text-[11px] text-gray-300 flex items-center justify-between gap-2 text-left transition cursor-pointer"
                >
                  <span>nujinagent onboard</span>
                  <span class="text-[10px] shrink-0 px-1.5 py-0.5 rounded bg-gray-800 text-gray-300">
                    {{ copiedCmd === 'nujinagent onboard' ? 'Copied!' : 'Copy' }}
                  </span>
                </button>
              </div>
            </div>

            <!-- Step 3 -->
            <div class="p-4 rounded-xl bg-black/60 border border-gray-800 space-y-2.5">
              <div class="text-xs font-mono text-emerald-400">03 · Speak Naturally</div>
              <div class="text-[11px] text-gray-400 leading-relaxed">
                <span class="text-gray-200 font-medium">WebUI:</span> Chat in your browser and state your trading goal in plain English.
              </div>
              <div>
                <div class="text-[11px] text-gray-400 mb-1"><span class="text-gray-200 font-medium">CLI:</span> State your trading goal in plain English via:</div>
                <button
                  type="button"
                  @click="copyCommand('nujinagent agent')"
                  class="w-full p-2 bg-black/90 rounded-lg border border-gray-800 hover:border-gray-600 font-mono text-[11px] text-gray-300 flex items-center justify-between gap-2 text-left transition cursor-pointer"
                >
                  <span>nujinagent agent</span>
                  <span class="text-[10px] shrink-0 px-1.5 py-0.5 rounded bg-gray-800 text-gray-300">
                    {{ copiedCmd === 'nujinagent agent' ? 'Copied!' : 'Copy' }}
                  </span>
                </button>
              </div>
            </div>
          </div>
        </div>

        <div class="mt-10 text-center" data-aos="fade-up">
          <div class="inline-block max-w-3xl px-6 py-4 rounded-2xl bg-emerald-950/40 border border-emerald-500/30 text-xs sm:text-sm text-gray-300 font-inter leading-relaxed">
            💡 <span class="font-semibold text-white">Pro Tip — Live Paper Trading in the Background:</span> Ask your AI agent to run your strategy as a live paper-trading bot in the background—and even set it to auto-start after device reboots—so you can track real live results over days without risking real money.
          </div>
        </div>
      </div>
    </section>

    <!-- Product Roadmap Section -->
    <section id="roadmap" class="py-24 border-t border-gray-900/80 bg-gradient-to-b from-black via-gray-950 to-black">
      <div class="px-6 sm:px-14 lg:px-24 xl:px-40">
        <div class="text-center max-w-3xl mx-auto mb-16" data-aos="fade-up">
          <span class="text-xs font-mono uppercase tracking-widest text-emerald-400">Ecosystem Roadmap</span>
          <h2 class="text-3xl sm:text-5xl font-bold text-white font-rubik mt-2">
            The NujinAI Horizon
          </h2>
          <p class="text-gray-400 pt-4 text-sm sm:text-base">
            Starting with universal agent skills, evolving into dedicated quant agents, desktop control centers, native OS environments, and handheld hardware.
          </p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-5 gap-4" data-aos="fade-up">
          <!-- Item 1: nujinSkill -->
          <a
            href="https://github.com/nujinAI/nujinSkills"
            target="_blank"
            class="p-6 rounded-2xl bg-gradient-to-b from-gray-900/90 to-gray-950 border-2 border-emerald-500/60 hover:border-emerald-400 shadow-lg shadow-emerald-950/30 relative flex flex-col justify-between transition"
          >
            <div>
              <div class="flex items-center justify-between mb-4">
                <span class="text-2xl">⚡</span>
                <span class="text-xs font-mono px-2.5 py-1 rounded-full bg-emerald-500/20 text-emerald-300 font-bold border border-emerald-500/30">Available Now</span>
              </div>
              <h3 class="text-lg font-bold text-white font-rubik font-mono">nujinSkill</h3>
              <p class="text-xs text-gray-400 pt-2 leading-relaxed">
                Universal AI agent skill compatible with any modern AI agent. 100% CPU-native quantitative anomaly discovery, 8-Gate audit, and strategy deployment.
              </p>
            </div>
            <div class="pt-6 flex items-center justify-between">
              <span class="text-xs font-mono text-emerald-400">Agent Skill</span>
              <span class="text-xs font-mono text-emerald-400">GitHub →</span>
            </div>
          </a>

          <!-- Item 2: nujinAgent -->
          <a
            href="https://github.com/nujinAI/nujinAgent"
            target="_blank"
            class="p-6 rounded-2xl bg-gradient-to-b from-gray-900/90 to-gray-950 border-2 border-emerald-500/60 hover:border-emerald-400 shadow-lg shadow-emerald-950/30 relative flex flex-col justify-between transition"
          >
            <div>
              <div class="flex items-center justify-between mb-4">
                <span class="text-2xl">🤖</span>
                <span class="text-xs font-mono px-2.5 py-1 rounded-full bg-emerald-500/20 text-emerald-300 font-bold border border-emerald-500/30">Available Now</span>
              </div>
              <h3 class="text-lg font-bold text-white font-rubik font-mono">nujinAgent</h3>
              <p class="text-xs text-gray-400 pt-2 leading-relaxed">
                Dedicated autonomous AI agent with nujinSkill preinstalled. Available via CLI &amp; WebUI for continuous research, execution, and self-auditing.
              </p>
            </div>
            <div class="pt-6 flex items-center justify-between">
              <span class="text-xs font-mono text-emerald-400">CLI &amp; WebUI</span>
              <span class="text-xs font-mono text-emerald-400">GitHub →</span>
            </div>
          </a>

          <!-- Item 3: nujinDesktop -->
          <div class="p-6 rounded-2xl bg-gray-900/60 border border-gray-800 hover:border-gray-700 transition flex flex-col justify-between">
            <div>
              <div class="flex items-center justify-between mb-4">
                <span class="text-2xl">🖥️</span>
                <span class="text-xs font-mono px-2.5 py-1 rounded-full bg-purple-500/10 text-purple-300 border border-purple-500/20">In Development</span>
              </div>
              <h3 class="text-lg font-bold text-white font-rubik font-mono">nujinDesktop</h3>
              <p class="text-xs text-gray-400 pt-2 leading-relaxed">
                Native institutional cockpit application. Multi-broker execution terminals, real-time strategy visualizer, Monte Carlo stress testing suite.
              </p>
            </div>
            <div class="pt-6">
              <span class="text-xs font-mono text-gray-500">macOS • Linux • Windows</span>
            </div>
          </div>

          <!-- Item 4: nujinOS -->
          <div class="p-6 rounded-2xl bg-gray-900/60 border border-gray-800 hover:border-gray-700 transition flex flex-col justify-between">
            <div>
              <div class="flex items-center justify-between mb-4">
                <span class="text-2xl">💾</span>
                <span class="text-xs font-mono px-2.5 py-1 rounded-full bg-indigo-500/10 text-indigo-300 border border-indigo-500/20">Roadmap</span>
              </div>
              <h3 class="text-lg font-bold text-white font-rubik font-mono">nujinOS</h3>
              <p class="text-xs text-gray-400 pt-2 leading-relaxed">
                Specialized Linux distribution fine-tuned for deterministic, low-latency agent orchestration, local LLM serving, and isolated sandboxed execution.
              </p>
            </div>
            <div class="pt-6">
              <span class="text-xs font-mono text-gray-500">Hardened Linux Distro</span>
            </div>
          </div>

          <!-- Item 5: nujinPhone -->
          <div class="p-6 rounded-2xl bg-gray-900/60 border border-gray-800 hover:border-gray-700 transition flex flex-col justify-between">
            <div>
              <div class="flex items-center justify-between mb-4">
                <span class="text-2xl">📱</span>
                <span class="text-xs font-mono px-2.5 py-1 rounded-full bg-pink-500/10 text-pink-300 border border-pink-500/20">Concept</span>
              </div>
              <h3 class="text-lg font-bold text-white font-rubik font-mono">nujinPhone</h3>
              <p class="text-xs text-gray-400 pt-2 leading-relaxed">
                Dedicated portable hardware running local secure quant models, real-time anomaly alerts, and hardware-secured private execution keys.
              </p>
            </div>
            <div class="pt-6">
              <span class="text-xs font-mono text-gray-500">Hardware Terminal</span>
            </div>
          </div>
        </div>
      </div>
    </section>

    <!-- Call to Action Section -->
    <section id="community" class="py-24 border-t border-gray-900/80 bg-gradient-to-b from-gray-950 to-black">
      <div class="px-6 sm:px-14 lg:px-24 xl:px-40">
        <div class="max-w-4xl mx-auto rounded-3xl bg-gradient-to-r from-gray-900 via-gray-900/90 to-emerald-950/40 border border-gray-800 p-8 sm:p-14 shadow-2xl relative overflow-hidden text-center sm:text-left" data-aos="zoom-in">
          <div class="max-w-2xl">
            <span class="text-xs font-mono uppercase tracking-widest text-emerald-400">Join the Quantitative AI Movement</span>
            <h2 class="text-3xl sm:text-5xl font-bold text-white font-rubik mt-2">
              Become Part of the Financial Revolution
            </h2>
            <p class="text-gray-300 text-base sm:text-lg pt-4 leading-relaxed">
              NujinAI is 100% open-source and free—no paywalls, no courses, no upsells. This is pure conviction to make financial freedom accessible and build a better world. Contributions are very welcome, and every contributor is credited in our repositories and listed right here on the page below.
            </p>
            <p class="text-xs text-gray-500 pt-2 font-mono">
              Open Source • Community Driven • 100% Free
            </p>

            <div class="pt-8 flex flex-col sm:flex-row items-stretch sm:items-center gap-4">
              <a
                href="https://www.linkedin.com/company/nujinai/"
                target="_blank"
                class="px-8 py-4 font-bold text-white bg-gradient-to-r from-fuchsia-500 via-pink-500 to-purple-600 animate-gradient-btn hover:opacity-90 rounded-xl whitespace-nowrap transition inline-flex items-center justify-center space-x-3 text-base"
              >
                <svg xmlns="http://www.w3.org/2000/svg" width="22" height="22" viewBox="0 0 20 20" fill="currentColor">
                  <path d="M17.04 17.043h-2.962v-4.64c0-1.107-.023-2.531-1.544-2.531c-1.544 0-1.78 1.204-1.78 2.449v4.722H7.793V7.5h2.844v1.3h.039c.397-.75 1.364-1.54 2.808-1.54c3.001 0 3.556 1.974 3.556 4.545zM4.447 6.194c-.954 0-1.72-.771-1.72-1.72s.767-1.72 1.72-1.72a1.72 1.72 0 0 1 0 3.44m1.484 10.85h-2.97V7.5h2.97zM18.522 0H1.476C.66 0 0 .645 0 1.44v17.12C0 19.355.66 20 1.476 20h17.042c.815 0 1.482-.644 1.482-1.44V1.44C20 .646 19.333 0 18.518 0z"/>
                </svg>
                <span>Follow on LinkedIn</span>
              </a>
              <a
                href="https://github.com/nujinAI/nujinSkills"
                target="_blank"
                class="px-6 py-4 font-semibold text-gray-300 hover:text-white border border-gray-700 bg-gray-900/60 rounded-xl whitespace-nowrap inline-flex items-center justify-center space-x-2 transition"
              >
                <span>Star GitHub Repository &rarr;</span>
              </a>
            </div>
            <div class="pt-4">
              <a
                href="https://github.com/christonomous/CodeCapitalFreedom"
                target="_blank"
                rel="noopener noreferrer"
                class="px-6 py-4 font-semibold text-emerald-400 hover:text-emerald-300 border border-emerald-500/40 bg-emerald-950/30 rounded-xl whitespace-nowrap inline-flex items-center justify-center space-x-2 transition"
              >
                <span>Read the Book: Code, Capital &amp; Freedom &rarr;</span>
              </a>
            </div>
          </div>
        </div>

        <!-- Contributors -->
        <div id="contributors" class="max-w-4xl mx-auto mt-12 text-center" data-aos="fade-up">
          <span class="text-xs font-mono uppercase tracking-widest text-emerald-400">Contributors</span>
          <p class="text-sm text-gray-400 mt-2">
            Built with the community. Every merged contributor is credited in our repositories and here.
          </p>
          <div class="mt-6 rounded-2xl border border-gray-800/80 bg-gray-900/40 px-6 py-6">
            <div v-if="contributors.length" class="flex flex-wrap items-center justify-center gap-3">
              <a
                v-for="c in contributors"
                :key="c.login"
                :href="c.html_url"
                target="_blank"
                rel="noopener noreferrer"
                class="inline-flex items-center gap-2.5 px-3.5 py-2 rounded-full border border-gray-800 bg-gray-950/70 hover:border-emerald-500/50 hover:text-white text-gray-300 text-sm transition"
              >
                <img
                  :src="c.avatar_url"
                  :alt="c.login"
                  class="w-6 h-6 rounded-full object-cover"
                  loading="lazy"
                />
                <span class="font-mono text-xs">@{{ c.login }}</span>
              </a>
            </div>
            <p class="text-xs text-gray-400" :class="contributors.length ? 'mt-5' : 'text-sm'">
              {{ contributors.length ? 'Want to join the list?' : 'Be the first to contribute —' }}
              read
              <a
                href="https://github.com/nujinAI/nujinSkills/blob/main/CONTRIBUTING.md"
                target="_blank"
                rel="noopener noreferrer"
                class="text-emerald-400 hover:text-emerald-300 underline underline-offset-4 transition"
              >
                CONTRIBUTING.md
              </a>
              to get started.
            </p>
          </div>
        </div>
      </div>
    </section>
  </div>
</template>

<style>
/* Smooth typography adjustments */
</style>