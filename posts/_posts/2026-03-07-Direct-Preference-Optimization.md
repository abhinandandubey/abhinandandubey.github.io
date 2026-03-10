---
layout: post
title: Direct Preference Optimization
tags: AI Machine-Learning sc
cover_url: https://abhinandandubey.github.io/posts/assets/images/dpo-cover.jpg
cover_meta: Volubilis (c) AD Photography
color_scheme: tango
mathjax: true
---
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css" crossorigin="anonymous">
<script src="/mini-coi.js" scope="/"></script>
<style>
.katex { font-size: 1.05em; color: #1a1a1a; }
.ke .katex { font-size: 0.95em; color: #1a1a1a; }
.kd .katex { font-size: 1.1em; color: #1a1a1a; }
</style>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js" crossorigin="anonymous"></script>
<link rel="stylesheet" href="https://pyscript.net/releases/2026.3.1/core.css">
<script>
// Prevent PyScript's py-editor from stealing scroll position on load
(function(){
  var _scrollSaved = null;
  var _origFocus = HTMLElement.prototype.focus;
  var _blockFocus = false;
  HTMLElement.prototype.focus = function(opts) {
    if (_blockFocus && this.closest && this.closest('py-editor, .py-editor, .cm-editor')) return;
    return _origFocus.call(this, opts);
  };
  _blockFocus = true;
  window.addEventListener('load', function() {
    setTimeout(function(){ _blockFocus = false; }, 3000);
  });
})();
</script>
<script type="module" src="https://pyscript.net/releases/2026.3.1/core.js"></script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>

<style>
.wrapper { max-width: 860px !important; }
article.post .post-content pre,
article .post-content pre,
article pre,
.post-content pre {
    max-width: 100% !important;
    overflow-x: auto !important;
    margin-left: 0 !important;
    margin-right: 0 !important;
    padding-left: 1rem !important;
    padding-right: 1rem !important;
    box-sizing: border-box !important;
}
article.post .post-content .highlighter-rouge,
article .post-content .highlighter-rouge,
.post-content .highlighter-rouge,
.post-content .highlight {
    margin-left: 0 !important;
    margin-right: 0 !important;
    padding-left: 0 !important;
    padding-right: 0 !important;
}
.post-content .highlight code {
    display: block;
    overflow-x: auto;
    font-size: 0.85rem;
    line-height: 1.5;
}
.post-content p { margin-bottom: 0.8rem; margin-top: 0; }
.post-content h1 {
    border-bottom: 1px solid #c4b8a8;
    padding-bottom: 0.35rem;
    margin-top: 2.5rem;
    margin-bottom: 0.85rem;
}
.post-content h2 {
    margin-top: 1.8rem;
    margin-bottom: 0.6rem;
}
</style>
<style>
.post-content blockquote {
    border-left: 2px solid #8c7b6b;
    background: transparent;
    padding: 0.3rem 1rem;
    margin: 0.5rem 0;
    font-size: 1.05rem;
    color: #4a4a4a;
    font-style: italic;
}
.post-content blockquote p { margin-bottom: 0.3rem; }
.defs ul { list-style: none; padding-left: 0; margin: 0.5rem 0; }
.defs li { margin-bottom: 0.6rem; padding-left: 1.2rem; text-indent: -1.2rem; }
.defs li:before { content: "·"; font-weight: bold; color: #8c7b6b; margin-right: 0.5rem; }
.cancel-box {
    border: 1px dashed #8c7b6b;
    padding: 0.75rem 1rem;
    margin: 0.75rem 0;
    border-radius: 4px;
    background: #f3efe9;
}
.cancel-box p { margin-bottom: 0.3rem !important; }
.pipeline {
    display: flex;
    align-items: center;
    gap: 0.4rem;
    flex-wrap: wrap;
    margin: 0.5rem 0;
    font-family: 'Inter', sans-serif;
    font-size: 0.8rem;
}
.pipeline .box {
    background: #ece7df;
    border: 1px solid #c4b8a8;
    padding: 0.35rem 0.65rem;
    border-radius: 4px;
    white-space: nowrap;
    color: #1a1a1a;
}
.pipeline .arrow { color: #8c7b6b; font-weight: bold; }
.vs-label {
    font-family: 'Inter', sans-serif;
    font-weight: 600;
    color: #1a1a1a;
    margin: 1rem 0 0.2rem;
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 0.12em;
}
</style>
<style>
.step-line {
    display: block;
    margin: 0.3rem 0;
    line-height: 1.6;
}
.step-num {
    display: inline-block;
    background: #1a1a1a;
    color: #f8f5f0;
    width: 20px; height: 20px;
    border-radius: 50%;
    text-align: center;
    line-height: 20px;
    font-size: 0.65rem;
    font-weight: 600;
    margin-right: 0.25rem;
    font-family: 'Inter', sans-serif;
    vertical-align: middle;
}
.interactive-box {
    background: #f3efe9;
    border: 1px solid #c4b8a8;
    border-radius: 6px;
    padding: 1.25rem;
    margin: 1.25rem 0;
    font-family: 'Inter', sans-serif;
    font-size: 13px;
}
.interactive-box .box-title {
    margin: 0 0 0.6rem 0;
    font-weight: 600;
    color: #1a1a1a;
    font-size: 0.65rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
}
.interactive-box .box-desc { margin: 0 0 0.6rem; color: #6b6b6b; font-size: 12px; }
.slider-row { display: flex; align-items: center; gap: 10px; margin: 0.4rem 0; }
.slider-row label { min-width: 100px; font-size: 12px; color: #4a4a4a; }
.slider-row input[type="range"] { flex: 1; accent-color: #8c7b6b; }
.slider-row .val { min-width: 50px; text-align: right; font-family: monospace; font-size: 12px; color: #1a1a1a; font-weight: 600; }
.result-row { margin-top: 0.6rem; padding-top: 0.6rem; border-top: 1px solid #c4b8a8; font-size: 13px; color: #1a1a1a; }
.result-row .result-val { font-family: monospace; font-weight: 700; color: #8c7b6b; font-size: 14px; }
.boost-table { width: 100%; border-collapse: collapse; margin-top: 0.4rem; font-size: 12px; }
.boost-table th, .boost-table td { padding: 4px 8px; border: 1px solid #c4b8a8; text-align: right; }
.boost-table th { background: #ece7df; font-weight: 600; text-align: center; font-size: 10px; text-transform: uppercase; letter-spacing: 0.05em; color: #1a1a1a; }
.boost-table td:first-child { text-align: left; }
canvas.widget-canvas { display: block; margin: 0.5rem auto 0; border: 1px solid #c4b8a8; border-radius: 3px; background: #faf8f4; }
</style>

<div style="margin-top: 2.5rem;"></div>

Direct Preference Optimization (DPO) is a technique for aligning language models with human preferences, without needing reinforcement learning. It replaces the traditional RLHF pipeline with a single supervised fine-tuning step and a clever loss function.

# Overview

Imagine you've built a chatbot that can write text, but it sometimes says unhelpful or weird things. You want to teach it to respond the way humans actually prefer. The question is: how do you do that efficiently?

<div class="interactive-box" id="llm-demo">
<div class="box-title">LLM Response Generation</div>
<div class="box-desc">Pick a prompt and watch the model generate two candidate responses, token by token. A human then picks the preferred one. This is how preference data is collected.</div>
<div style="margin-bottom:0.6rem;">
  <button class="llm-prompt-btn" data-idx="0" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 10px;border:1px solid #1a1a1a;background:#1a1a1a;color:#f8f5f0;border-radius:3px;cursor:pointer;margin-right:4px;">Explain gravity</button>
  <button class="llm-prompt-btn" data-idx="1" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 10px;border:1px solid #c4b8a8;background:#f3efe9;color:#1a1a1a;border-radius:3px;cursor:pointer;margin-right:4px;">Write a poem</button>
  <button class="llm-prompt-btn" data-idx="2" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 10px;border:1px solid #c4b8a8;background:#f3efe9;color:#1a1a1a;border-radius:3px;cursor:pointer;">Summarize ML</button>
</div>
<div style="background:#1e1e2e;border-radius:4px;padding:0.75rem 1rem;margin-bottom:0.5rem;font-family:Menlo,Consolas,monospace;font-size:12px;">
  <div style="color:#6c7086;margin-bottom:0.4rem;">Prompt:</div>
  <div id="llm-prompt" style="color:#cdd6f4;margin-bottom:0.7rem;min-height:1.2em;"></div>
  <div style="display:flex;gap:1rem;flex-wrap:wrap;">
    <div style="flex:1;min-width:200px;">
      <div style="color:#a6e3a1;font-size:10px;text-transform:uppercase;letter-spacing:0.1em;margin-bottom:0.3rem;">Response A</div>
      <div id="llm-resp-a" style="color:#cdd6f4;min-height:3em;line-height:1.5;"></div>
    </div>
    <div style="flex:1;min-width:200px;">
      <div style="color:#f38ba8;font-size:10px;text-transform:uppercase;letter-spacing:0.1em;margin-bottom:0.3rem;">Response B</div>
      <div id="llm-resp-b" style="color:#cdd6f4;min-height:3em;line-height:1.5;"></div>
    </div>
  </div>
</div>
<div id="llm-verdict" style="font-size:12px;color:#777;min-height:2.5em;margin-top:0.3rem;"></div>
</div>

The old way (RLHF) had three complicated steps. First, you show humans two responses to the same question and ask "which one is better?" to collect preference data. Second, you build a separate "judge" model (a reward model) that learns to score responses the way humans would. Third, you use reinforcement learning to nudge your chatbot toward getting higher scores from that judge. This whole pipeline is expensive, fragile, and hard to get right.

The DPO breakthrough: the authors discovered a mathematical shortcut. They showed that you can collapse all three steps into one. Instead of building a separate judge and then doing the complicated RL dance, you can directly adjust the chatbot using the human preference data alone. Skip the middleman. The training becomes as simple as "make the preferred response more likely and the dispreferred response less likely," with some clever math to keep things stable.

Before DPO, aligning a chatbot with human preferences required a complicated three-stage pipeline. DPO replaced it with a single, simple training step that works just as well or better. That's why it became so widely adopted so quickly.

<canvas id="sketch-pipeline" width="780" height="340" style="display:block;margin:1.5rem auto;max-width:100%;border-radius:6px;background:#f3efe9;"></canvas>

# The Cast of Characters

<div class="defs">
<ul>
<li><span class="ke">\pi_{ref}(y \mid x)</span>, <b>the Reference Model.</b> A frozen snapshot of the chatbot taken before training starts. It acts like a safety anchor: during training, we say "you can improve, but don't drift too far from how you originally behaved." This prevents the model from going off the rails. <span class="ke">\pi_{ref}(y \mid x)</span> is the probability this model assigns to generating response <span class="ke">y</span> given prompt <span class="ke">x</span>. For example, given prompt <span class="ke">x</span> = "What's the capital of France?": <span class="ke">\pi_{ref}(\text{"Paris"} \mid x) = 0.4</span>, <span class="ke">\pi_{ref}(\text{"The capital is Paris."} \mid x) = 0.3</span>, <span class="ke">\pi_{ref}(\text{"I like cheese"} \mid x) = 0.001</span>.</li>
<li><span class="ke">\pi_\theta(y \mid x)</span>, <b>the Policy (the Model We're Training).</b> "Policy" just means "the chatbot and how it decides what to say next." When we "optimize the policy," we're just making the chatbot better at responding. Same architecture as the reference, but these are the weights <span class="ke">\theta</span> we update. Starts as a copy of <span class="ke">\pi_{ref}</span> and gradually changes.</li>
<li><span class="ke">r(x, y)</span>, <b>the Reward.</b> A scalar: how good is response <span class="ke">y</span> for prompt <span class="ke">x</span>. Higher is better. In RLHF, a separate AI (the reward model) plays judge at a talent show, rating outputs as good or bad. DPO's big move is eliminating this entirely.</li>
<li><span class="ke">\beta</span>, <b>the Leash.</b> A hyperparameter (e.g. 0.1 or 0.5) controlling how far the trained model can stray from the reference. High <span class="ke">\beta</span> = stay close. Low <span class="ke">\beta</span> = chase reward.</li>
<li><span class="ke">D_{KL}</span>, <b>KL Divergence.</b> A way to measure how different two distributions are. Here it measures how far the chatbot has drifted from its original self. DPO uses this as a leash: if the model strays too far, the math pulls it back.</li>
<li><span class="ke">\sigma(\cdot)</span>, <b>the Sigmoid.</b> Squashes any number to <span class="ke">[0, 1]</span>. <span class="ke">\sigma(z) = \frac{1}{1 + e^{-z}}</span>.</li>
<li><span class="ke">y_w</span> and <span class="ke">y_l</span>, <b>Winner and Loser.</b> <span class="ke">y_w</span> is the human-preferred response, <span class="ke">y_l</span> is the rejected one.</li>
<li><span class="ke">\succ</span>, <b>"is preferred to."</b> <span class="ke">p(A \succ B)</span> = probability that A beats B.</li>
</ul>
</div>

<div class="interactive-box" id="training-sim">
<div class="box-title">DPO Training Simulator</div>
<div class="box-desc">A model is asked "Summarize this article." It can produce three responses. The human prefers the concise summary (winner) over the verbose one (loser). Watch how DPO shifts probability mass over gradient steps.</div>
<div class="slider-row">
  <label>β (leash)</label>
  <input type="range" id="sim-beta" min="0.1" max="2.0" step="0.1" value="0.5">
  <span class="val" id="sim-beta-val">0.5</span>
</div>
<div class="slider-row">
  <label>Learning rate</label>
  <input type="range" id="sim-lr" min="0.01" max="0.15" step="0.01" value="0.05">
  <span class="val" id="sim-lr-val">0.05</span>
</div>
<div style="margin:0.6rem 0 0.3rem;">
  <button id="sim-step" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 12px;border:1px solid #8c7b6b;background:#f3efe9;color:#1a1a1a;border-radius:3px;cursor:pointer;margin-right:6px;">Step</button>
  <button id="sim-run" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 12px;border:1px solid #8c7b6b;background:#f3efe9;color:#1a1a1a;border-radius:3px;cursor:pointer;margin-right:6px;">Run 50 steps</button>
  <button id="sim-reset" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 12px;border:1px solid #c4b8a8;background:#f3efe9;color:#6b6b6b;border-radius:3px;cursor:pointer;">Reset</button>
  <span style="margin-left:10px;font-size:11px;color:#888;">Step: <span id="sim-step-num">0</span></span>
</div>
<canvas id="sim-canvas" width="560" height="200" style="display:block;margin:0.5rem auto 0;border:1px solid #c4b8a8;border-radius:3px;background:#faf8f4;width:100%;"></canvas>
<div style="margin-top:0.5rem;font-size:11px;color:#777;">
  <span style="display:inline-block;width:10px;height:10px;background:#8c7b6b;border-radius:2px;vertical-align:middle;margin-right:3px;"></span> π_θ (current)
  <span style="display:inline-block;width:10px;height:10px;background:#c4b8a8;border-radius:2px;vertical-align:middle;margin-left:12px;margin-right:3px;"></span> π_ref (frozen)
</div>
<div class="result-row">
  Loss = <span class="result-val" id="sim-loss"></span>
  &emsp; p(winner ≻ loser) = <span class="result-val" id="sim-pref"></span>
</div>
</div>

# The Goal

Find parameters <span class="ke">\theta</span> that maximize expected reward:

<div class="kd">\max_\theta \; \mathbb{E}_{x \sim D,\, y \sim \pi_\theta(y \mid x)} \big[ r(x, y) \big]</div>

The problem: without constraints, the model games the reward. If the reward model likes responses starting with "ABSOLUTELY! GREAT QUESTION!", the LLM learns to say that every time.

# The KL Constraint

Fix: penalize the model for straying too far from the reference. The KL divergence acts as a leash: if the model drifts too far from its original self, the math pulls it back.

<div class="kd">\max_\theta \; \mathbb{E}\big[ r(x, y) \big] \;-\; \beta \, D_{KL}\big[\pi_\theta(y \mid x) \,\|\, \pi_{ref}(y \mid x)\big]</div>

<span class="ke">D_{KL}[P \| Q]</span> (KL Divergence) measures how different two distributions are. Zero when identical, larger as they diverge.

# The Optimal Policy

Instead of searching for the answer through thousands of trial-and-error steps (which is what RL does), the math gives you a direct formula. It's the difference between solving an equation on paper versus guessing and checking repeatedly. The closed-form solution to the constrained objective:

<div class="kd">\pi^*(y \mid x) = \frac{1}{Z(x)} \; \pi_{ref}(y \mid x) \cdot \exp\!\Big(\frac{r(x,y)}{\beta}\Big)</div>

For each response, take the reference probability and multiply by a boost factor <span class="ke">\exp(r/\beta)</span>. High reward = boost. Low reward = shrink. Then normalize by <span class="ke">Z(x)</span>.

## The Boost Factor

<div class="interactive-box" id="boost-demo">
<div class="box-title">Boost Factor</div>
<div class="box-desc">Adjust β and see how the optimal policy redistributes probability mass.</div>
<div class="slider-row">
  <label>β</label>
  <input type="range" id="boost-beta" min="0.1" max="3.0" step="0.1" value="1.0">
  <span class="val" id="boost-beta-val">1.0</span>
</div>
<table class="boost-table">
  <tr><th>Response</th><th>π_ref</th><th>Reward</th><th>Boost</th><th>π*</th></tr>
  <tr><td>"The capital is Paris."</td><td>0.30</td><td>2.5</td><td id="b1"></td><td id="p1"></td></tr>
  <tr><td>"Paris"</td><td>0.40</td><td>2.0</td><td id="b2"></td><td id="p2"></td></tr>
  <tr><td>"I like cheese"</td><td>0.001</td><td>-1.0</td><td id="b3"></td><td id="p3"></td></tr>
</table>
<div class="result-row">Z(x) = <span class="result-val" id="boost-z"></span></div>
</div>

<span class="ke">Z(x) = \sum_{y} \pi_{ref}(y \mid x) \cdot \exp(r(x,y)/\beta)</span> sums over every possible string the LLM could produce. That's infinite. You can't compute it directly.

# The Rearrangement Trick

Rearrange the optimal policy equation to express reward in terms of the policy:

<div class="kd">r(x, y) = \beta \log \frac{\pi^*(y \mid x)}{\pi_{ref}(y \mid x)} + \beta \log Z(x)</div>

The reward equals how much the optimal model prefers a response *relative to* the reference (times <span class="ke">\beta</span>), plus a constant.

# The Bradley-Terry Model

Before we get to the cancellation, we need to talk about how we model human preferences. The Bradley-Terry model is surprisingly simple. It was introduced in 1952 by Ralph Bradley and Milton Terry as a way to rank items from pairwise comparisons. Think chess ratings, taste tests, or any setting where you compare two things and pick a winner.

The idea: each item <span class="ke">i</span> has a latent "strength" <span class="ke">s_i</span>. The probability that item <span class="ke">i</span> beats item <span class="ke">j</span> is:

<div class="kd">p(i \succ j) = \frac{s_i}{s_i + s_j}</div>

If you parameterize strengths as exponentials of scores, <span class="ke">s_i = e^{r_i}</span>, this becomes:

<div class="kd">p(i \succ j) = \frac{e^{r_i}}{e^{r_i} + e^{r_j}} = \frac{1}{1 + e^{-(r_i - r_j)}} = \sigma(r_i - r_j)</div>

That's it. The probability that <span class="ke">i</span> beats <span class="ke">j</span> is just the sigmoid of the difference in their scores. The formula is simple on purpose. The power is in the statistical machinery for fitting it to messy, incomplete real-world comparison data.

## Why Bradley-Terry Matters

The value isn't in the formula itself. Imagine you have 100 chess players and a messy pile of game results where not everyone has played everyone. Bradley-Terry gives you a principled way to estimate a single strength number for each player from incomplete pairwise comparisons using maximum likelihood estimation. You can then rank all 100 players on a single scale, even if player 1 never faced player 87.

That's surprisingly hard to do well without a model like this. Simple win percentages don't work because schedules differ: someone who only played weak opponents would look artificially strong. Elo ratings are actually a special case of Bradley-Terry, so if you've ever looked at chess ratings, you've already been using this model.

Applied to LLM alignment: given a prompt <span class="ke">x</span> and two responses <span class="ke">y_1, y_2</span> with rewards <span class="ke">r(x, y_1)</span> and <span class="ke">r(x, y_2)</span>:

<div class="kd">p(y_1 \succ y_2 \mid x) = \sigma\big(r(x, y_1) - r(x, y_2)\big)</div>

Human preference depends only on the *difference* in rewards. This is the property that makes DPO possible.

## The Reward Model Loss (What DPO Replaces)

In RLHF, you train a neural network <span class="ke">r_\phi</span> (where <span class="ke">\phi</span> are its learnable weights) to approximate the ideal reward function. The loss:

<div class="kd">\mathcal{L}_R(r_\phi, \mathcal{D}) = -\mathbb{E}_{(x, y_w, y_l) \sim \mathcal{D}}\big[\log \sigma(r_\phi(x, y_w) - r_\phi(x, y_l))\big]</div>

A concrete example. Say one data point is:

- Prompt <span class="ke">x</span>: "Explain gravity"
- <span class="ke">y_w</span> (winner): "Gravity is the force that attracts objects with mass toward each other"
- <span class="ke">y_l</span> (loser): "Gravity is like when stuff falls down because the earth is big"

The reward model scores both: <span class="ke">r_\phi(x, y_w) = 1.8</span>, <span class="ke">r_\phi(x, y_l) = 1.2</span>.

Then: difference = 0.6, <span class="ke">\sigma(0.6) = 0.645</span>, <span class="ke">\log(0.645) = -0.439</span>, negate: loss = 0.439.

If the model had given the winner a much higher score (say difference of 5), <span class="ke">\sigma(5) \approx 0.993</span>, <span class="ke">\log(0.993) \approx -0.007</span>, loss <span class="ke">\approx 0.007</span>. Much smaller. So the loss pushes the model to score winners well above losers.

The <span class="ke">\mathbb{E}</span> is just a fancy way of saying "average over the dataset." In practice it's literally <span class="ke">\frac{1}{N}\sum_{i=1}^{N}</span>. They use <span class="ke">\mathbb{E}</span> because it's more general, but mentally just read it as "average."

The negation: we want to *maximize* the log-likelihood (make the data as probable as possible). But every optimization framework (PyTorch, etc.) is set up to *minimize* a loss. So you slap a minus sign on it. Maximize <span class="ke">\log(\text{likelihood})</span> = minimize <span class="ke">-\log(\text{likelihood})</span>. Same thing, just a convention.

Think of each preference pair as a classification problem. For every <span class="ke">(x, y_w, y_l)</span>, you're asking: "which response is better?" The label is always <span class="ke">y_w</span> (by definition, it's the one the human picked). So <span class="ke">-\log(\sigma(\ldots))</span> is exactly binary cross-entropy loss when the true label is 1. If you've ever trained a logistic regression classifier, it's the same loss. The reward model is essentially a binary classifier that says "given two responses, which one is better?" and Bradley-Terry via the sigmoid is what connects the reward scores to that binary prediction.

This is the entire machinery that DPO eliminates.

# The Cancellation

Bradley-Terry preference modeling only uses the *difference* in rewards. When you subtract:

<div class="cancel-box">
<p>Reward for <span class="ke">y_1</span>: <span class="ke">\;\beta \log \frac{\pi^*(y_1 \mid x)}{\pi_{ref}(y_1 \mid x)} + \beta \log Z(x)</span></p>
<p>Reward for <span class="ke">y_2</span>: <span class="ke">\;\beta \log \frac{\pi^*(y_2 \mid x)}{\pi_{ref}(y_2 \mid x)} + \beta \log Z(x)</span></p>
<p style="font-weight:600; color:#8c7b6b; margin-top:0.3rem;">The <span class="ke">\beta \log Z(x)</span> is identical in both. It cancels.</p>
</div>

<div class="kd">p^*(y_1 \succ y_2 \mid x) = \sigma\!\left(\beta \log \frac{\pi^*(y_1 \mid x)}{\pi_{ref}(y_1 \mid x)} - \beta \log \frac{\pi^*(y_2 \mid x)}{\pi_{ref}(y_2 \mid x)}\right)</div>

No reward model. No intractable <span class="ke">Z(x)</span>. Just log-ratios of how the policy diverges from the reference.

<canvas id="sketch-cancel" width="780" height="280" style="display:block;margin:1.5rem auto;max-width:100%;border-radius:6px;background:#f3efe9;"></canvas>

# The DPO Loss

<div class="kd">L_{DPO}(\pi_\theta;\, \pi_{ref}) = -\mathbb{E}_{(x,\, y_w,\, y_l)\, \sim\, D}\!\left[\, \log \sigma\!\left(\, \beta \log \frac{\pi_\theta(y_w \mid x)}{\pi_{ref}(y_w \mid x)} - \beta \log \frac{\pi_\theta(y_l \mid x)}{\pi_{ref}(y_l \mid x)} \,\right) \,\right]</div>

<span class="step-line"><span class="step-num">1</span> <span class="ke">\pi_\theta(y_w \mid x) / \pi_{ref}(y_w \mid x)</span>: how much more likely is the winner under the new model vs the original?</span>
<span class="step-line"><span class="step-num">2</span> Same ratio for the loser.</span>
<span class="step-line"><span class="step-num">3</span> Subtract: we want the winner's ratio to exceed the loser's.</span>
<span class="step-line"><span class="step-num">4</span> <span class="ke">\sigma(\cdot)</span>: squash to a probability.</span>
<span class="step-line"><span class="step-num">5</span> <span class="ke">\log</span>: log-likelihood.</span>
<span class="step-line"><span class="step-num">6</span> <span class="ke">-\mathbb{E}</span>: negate and average.</span>

Training pushes the model to increase the probability of winners and decrease the probability of losers, relative to the reference.

<canvas id="sketch-dpo" width="780" height="260" style="display:block;margin:1.5rem auto;max-width:100%;border-radius:6px;background:#f3efe9;"></canvas>

<div class="interactive-box" id="dpo-demo">
<div class="box-title">DPO Loss Playground</div>
<div class="box-desc">Adjust the model's probabilities for the winner and loser. Reference probabilities are fixed.</div>
<div class="slider-row">
  <label>β</label>
  <input type="range" id="dpo-beta" min="0.1" max="2.0" step="0.05" value="0.5">
  <span class="val" id="dpo-beta-val">0.50</span>
</div>
<div class="slider-row">
  <label>π_θ(y_w|x)</label>
  <input type="range" id="dpo-pw" min="0.01" max="0.99" step="0.01" value="0.30">
  <span class="val" id="dpo-pw-val">0.30</span>
</div>
<div class="slider-row">
  <label>π_θ(y_l|x)</label>
  <input type="range" id="dpo-pl" min="0.01" max="0.99" step="0.01" value="0.20">
  <span class="val" id="dpo-pl-val">0.20</span>
</div>
<div style="font-size:11px; color:#999; margin:0.3rem 0;">Reference: π_ref(y_w|x) = 0.20, π_ref(y_l|x) = 0.25</div>
<div class="result-row">
  p(y_w ≻ y_l) = <span class="result-val" id="dpo-pref"></span>
  &emsp; Loss = <span class="result-val" id="dpo-loss"></span>
</div>
<div style="margin-top:0.3rem; font-size:11px; color:#999;"><span id="dpo-hint"></span></div>
</div>

# Why Wasn't DPO Obvious?

If the math is this clean, why did the field spend years on RLHF before someone wrote down DPO? A few reasons.

The RLHF pipeline was built incrementally. Christiano et al. (2017) introduced learning rewards from human preferences. The natural next step was to use those rewards with RL, because that's what rewards are for. The pipeline worked: train a reward model, then run PPO against it. Each piece made sense on its own, and the combination produced real results. When something works, there's less pressure to ask whether a simpler path exists.

The key insight in DPO is that you can rearrange the closed-form optimal policy to express reward as a function of the policy itself, then substitute that into Bradley-Terry. This requires noticing that the intractable partition function <span class="ke">Z(x)</span> cancels when you only care about reward *differences*. That cancellation is obvious in hindsight, but it requires you to write down the optimal policy, solve for the reward, and then plug it into the preference model. Most researchers were thinking about the problem in the forward direction: given rewards, find the policy. DPO thinks backward: given the policy, what rewards does it imply?

There's also a conceptual barrier. RL from human feedback frames alignment as a sequential decision problem. DPO reframes it as supervised learning with a particular loss function. These are different mental models, and switching between them isn't trivial. The RL framing was dominant in the alignment community, and it took fresh eyes to see that the RL machinery was unnecessary for this specific problem.

Finally, the closed-form solution to the KL-constrained reward maximization was known in the RL literature (it appears in work on maximum entropy RL), but connecting it to preference learning and recognizing the <span class="ke">Z(x)</span> cancellation required combining ideas from different subfields. DPO sits at the intersection of preference learning, KL-regularized RL, and supervised fine-tuning. The pieces were all there; someone just had to put them together.

# Implementing DPO in Python

The loss function is simple enough to implement from scratch. Here's a minimal version using PyTorch.

<pre style="background:#f3efe9;border:1px solid #c4b8a8;border-radius:4px;padding:1rem;overflow-x:auto;font-size:0.82rem;line-height:1.55;margin:0.75rem 0;"><code style="font-family:Menlo,Consolas,monospace;color:#1a1a1a;">import torch
import torch.nn.functional as F

def dpo_loss(pi_lp_w, pi_lp_l, ref_lp_w, ref_lp_l, beta=0.1):
    log_ratio_w = pi_lp_w - ref_lp_w
    log_ratio_l = pi_lp_l - ref_lp_l
    logits = beta * (log_ratio_w - log_ratio_l)
    return -F.logsigmoid(logits).mean()</code></pre>

That's the entire loss. Four lines of math. The rest is plumbing: computing log-probabilities from a language model, loading preference data, and running a training loop.

Below is a runnable version using only NumPy (so it works in the browser). It trains a toy model on preference pairs and prints how the probability distribution shifts. Click the green play button to run it.

<script type="py-editor" config='{"packages": ["numpy"]}'>
import numpy as np

def softmax(x):
    e = np.exp(x - x.max(axis=-1, keepdims=True))
    return e / e.sum(axis=-1, keepdims=True)

def log_softmax(x):
    return x - np.log(np.exp(x).sum(axis=-1, keepdims=True))

def sigmoid(x):
    return 1.0 / (1.0 + np.exp(-x))

# Toy model: 3 prompts, 4 possible responses each
np.random.seed(42)
ref_logits = np.random.randn(3, 4) * 0.1
policy_logits = ref_logits.copy()

# Preference data: (prompt, winner, loser)
data = [(0,2,1),(0,2,3),(1,0,2),(1,0,3),(2,1,0),(2,1,3)]
prompts = np.array([d[0] for d in data])
winners = np.array([d[1] for d in data])
losers  = np.array([d[2] for d in data])

beta = 0.5
lr = 0.05

print("Training DPO on toy preference data...\n")
for step in range(200):
    # Log-probs under current policy and frozen reference
    pi_lp = log_softmax(policy_logits)
    ref_lp = log_softmax(ref_logits)

    pi_lp_w = pi_lp[prompts, winners]
    pi_lp_l = pi_lp[prompts, losers]
    ref_lp_w = ref_lp[prompts, winners]
    ref_lp_l = ref_lp[prompts, losers]

    # DPO loss
    inside = beta * ((pi_lp_w - ref_lp_w) - (pi_lp_l - ref_lp_l))
    loss = -np.log(sigmoid(inside)).mean()

    # Gradient (manual, since no autograd)
    sig = sigmoid(inside)
    grad_scale = (1 - sig) * beta / len(data)
    probs = softmax(policy_logits)
    grad = np.zeros_like(policy_logits)
    for i, (p, w, l) in enumerate(data):
        grad[p, w] += grad_scale[i] * (1 - probs[p, w])
        grad[p, l] -= grad_scale[i] * (1 - probs[p, l])

    policy_logits += lr * grad

    if step % 50 == 0:
        print(f"Step {step:3d}  Loss: {loss:.4f}")

# Results
probs = softmax(policy_logits)
ref_probs = softmax(ref_logits)
print(f"\nPrompt 0 distribution:")
print(f"  Before DPO: {np.round(ref_probs[0], 3)}")
print(f"  After DPO:  {np.round(probs[0], 3)}")
print(f"  Winner (resp 2): {ref_probs[0,2]:.3f} → {probs[0,2]:.3f}")
print(f"  Loser  (resp 1): {ref_probs[0,1]:.3f} → {probs[0,1]:.3f}")
</script>

The winner's probability climbs while the loser drops. That's DPO doing its job: shifting probability mass toward preferred responses, constrained by the KL penalty against the reference.

A few things to note for real implementations:

- Log-probabilities for a full sequence are the sum of per-token log-probs: <span class="ke">\log \pi(y \mid x) = \sum_{t=1}^{T} \log \pi(y_t \mid x, y_{<t})</span>
- The reference model is typically a frozen copy of the model before DPO training
- <span class="ke">\beta</span> values between 0.1 and 0.5 are common in practice; lower values allow more aggressive optimization
- Libraries like TRL (Hugging Face) wrap all of this into a `DPOTrainer` class that handles tokenization, batching, and distributed training

# RLHF vs DPO

<div class="vs-label">RLHF (two stages)</div>
<div class="pipeline">
  <span class="box">Human preferences</span><span class="arrow">→</span><span class="box">Train reward model</span><span class="arrow">→</span><span class="box">Run RL (PPO)</span><span class="arrow">→</span><span class="box">Updated LLM</span>
</div>

<div class="vs-label">DPO (one stage)</div>
<div class="pipeline">
  <span class="box">Human preferences</span><span class="arrow">→</span><span class="box">Supervised fine-tuning with DPO loss</span><span class="arrow">→</span><span class="box">Updated LLM</span>
</div>

DPO skips the reward model and the RL loop entirely. Simpler, more stable, easier to implement.

<div class="interactive-box" id="beta-demo">
<div class="box-title">β Effect on Model Output</div>
<div class="box-desc">See how β (the KL leash) changes what the model generates. Low β lets the model aggressively chase the preferred style. High β keeps it close to the original (reference) behavior. The model has been trained on preference data that favors concise, direct answers.</div>
<div style="margin-bottom:0.6rem;">
  <button class="beta-prompt-btn" data-idx="0" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 10px;border:1px solid #1a1a1a;background:#1a1a1a;color:#f8f5f0;border-radius:3px;cursor:pointer;margin-right:4px;">Explain black holes</button>
  <button class="beta-prompt-btn" data-idx="1" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 10px;border:1px solid #c4b8a8;background:#f3efe9;color:#1a1a1a;border-radius:3px;cursor:pointer;margin-right:4px;">Recommend a book</button>
  <button class="beta-prompt-btn" data-idx="2" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 10px;border:1px solid #c4b8a8;background:#f3efe9;color:#1a1a1a;border-radius:3px;cursor:pointer;">Healthy breakfast</button>
</div>
<div class="slider-row">
  <label>β</label>
  <input type="range" id="beta-eff-slider" min="0" max="100" step="1" value="50">
  <span class="val" id="beta-eff-val">0.50</span>
</div>
<div style="display:flex;gap:0.5rem;font-family:Inter,sans-serif;font-size:10px;color:#999;margin-bottom:0.6rem;">
  <span>← low β (chase reward)</span><span style="margin-left:auto;">high β (stay safe) →</span>
</div>
<div style="background:#1e1e2e;border-radius:4px;padding:0.75rem 1rem;font-family:Menlo,Consolas,monospace;font-size:12px;">
  <div style="color:#6c7086;margin-bottom:0.3rem;">Prompt: <span id="beta-eff-prompt" style="color:#cdd6f4;"></span></div>
  <div style="display:flex;gap:1rem;margin-top:0.5rem;flex-wrap:wrap;">
    <div style="flex:1;min-width:200px;">
      <div style="color:#c4b8a8;font-size:10px;text-transform:uppercase;letter-spacing:0.1em;margin-bottom:0.3rem;">π_ref (reference model)</div>
      <div id="beta-eff-ref" style="color:#6c7086;min-height:3em;line-height:1.5;font-size:11px;"></div>
    </div>
    <div style="flex:1;min-width:200px;">
      <div style="color:#a6e3a1;font-size:10px;text-transform:uppercase;letter-spacing:0.1em;margin-bottom:0.3rem;">π_θ (DPO-trained at this β)</div>
      <div id="beta-eff-policy" style="color:#cdd6f4;min-height:3em;line-height:1.5;font-size:11px;"></div>
    </div>
  </div>
</div>
<div style="margin-top:0.5rem;display:flex;gap:1rem;flex-wrap:wrap;">
  <div style="flex:1;min-width:120px;background:#f3efe9;border-radius:4px;padding:0.5rem 0.75rem;text-align:center;">
    <div style="font-size:10px;color:#6b6b6b;text-transform:uppercase;letter-spacing:0.05em;">KL divergence</div>
    <div id="beta-eff-kl" style="font-family:monospace;font-size:16px;font-weight:700;color:#8c7b6b;margin-top:0.2rem;"></div>
  </div>
  <div style="flex:1;min-width:120px;background:#f3efe9;border-radius:4px;padding:0.5rem 0.75rem;text-align:center;">
    <div style="font-size:10px;color:#6b6b6b;text-transform:uppercase;letter-spacing:0.05em;">Reward gain</div>
    <div id="beta-eff-reward" style="font-family:monospace;font-size:16px;font-weight:700;color:#5a7a5a;margin-top:0.2rem;"></div>
  </div>
  <div style="flex:1;min-width:120px;background:#f3efe9;border-radius:4px;padding:0.5rem 0.75rem;text-align:center;">
    <div style="font-size:10px;color:#6b6b6b;text-transform:uppercase;letter-spacing:0.05em;">Objective (reward − β·KL)</div>
    <div id="beta-eff-obj" style="font-family:monospace;font-size:16px;font-weight:700;color:#1a1a1a;margin-top:0.2rem;"></div>
  </div>
</div>
<div id="beta-eff-note" style="font-size:11px;color:#999;margin-top:0.5rem;"></div>
</div>

# Caveats

DPO and RLHF are equivalent in theory, if Bradley-Terry perfectly captures human preferences and you find the global optimum. In practice they can differ: Bradley-Terry is an approximation, the learned reward isn't the true optimal, and gradient descent on a non-convex landscape doesn't find the global optimum. There's also an overfitting risk: if one response *always* wins in the data, DPO pushes the reward gap toward infinity, driving the loser's probability to zero.
