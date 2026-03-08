---
layout: post
title: Direct Preference Optimization
tags: AI Machine-Learning
color_scheme: tango
mathjax: true
---
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css" crossorigin="anonymous">
<style>
.katex { font-size: 1.05em; color: #3a3f52; }
.ke .katex { font-size: 0.95em; color: #4a5068; }
.kd .katex { font-size: 1.1em; color: #3a3f52; }
</style>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js" crossorigin="anonymous"></script>
<link rel="stylesheet" href="https://pyscript.net/releases/2024.1.1/core.css">
<script type="module" src="https://pyscript.net/releases/2024.1.1/core.js"></script>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,300;1,400;1,500&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">

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
.post-content {
    font-family: 'Cormorant Garamond', Georgia, serif;
    color: #2d3142;
    font-size: 1.15rem;
    line-height: 1.75;
}
.post-content p { margin-bottom: 0.6rem; margin-top: 0; }
.post-content h1 {
    font-family: 'Cormorant Garamond', Georgia, serif;
    color: #4a6fa5;
    font-weight: 500;
    border-bottom: 1.5px solid #ddd5cc;
    padding-bottom: 0.25rem;
    margin-top: 2rem;
    margin-bottom: 0.75rem;
    font-size: 1.6rem;
}
.post-content h2 {
    font-family: 'Cormorant Garamond', Georgia, serif;
    color: #4a6fa5;
    font-weight: 500;
    margin-top: 1.5rem;
    margin-bottom: 0.5rem;
    font-size: 1.3rem;
}
</style>
<style>
.post-content blockquote {
    border-left: 2px solid #c0c0c0;
    background: transparent;
    padding: 0.3rem 1rem;
    margin: 0.5rem 0;
    font-size: 1rem;
    color: #5a6178;
    font-style: italic;
}
.post-content blockquote p { margin-bottom: 0.3rem; }
.defs ul { list-style: none; padding-left: 0; margin: 0.5rem 0; }
.defs li { margin-bottom: 0.6rem; padding-left: 1.2rem; text-indent: -1.2rem; }
.defs li:before { content: "·"; font-weight: bold; color: #4a6fa5; margin-right: 0.5rem; }
.cancel-box {
    border: 1px dashed #999;
    padding: 0.75rem 1rem;
    margin: 0.75rem 0;
    border-radius: 4px;
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
    background: #f0eeeb;
    border: 1px solid #ddd5cc;
    padding: 0.35rem 0.65rem;
    border-radius: 4px;
    white-space: nowrap;
}
.pipeline .arrow { color: #4a6fa5; font-weight: bold; }
.vs-label {
    font-family: 'Inter', sans-serif;
    font-weight: 600;
    color: #4a6fa5;
    margin: 1rem 0 0.2rem;
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 0.1em;
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
    background: #4a6fa5;
    color: white;
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
    background: #f7f6f4;
    border: 1px solid #ddd5cc;
    border-radius: 6px;
    padding: 1.25rem;
    margin: 1.25rem 0;
    font-family: 'Inter', sans-serif;
    font-size: 13px;
}
.interactive-box .box-title {
    margin: 0 0 0.6rem 0;
    font-weight: 600;
    color: #4a6fa5;
    font-size: 0.65rem;
    letter-spacing: 0.15em;
    text-transform: uppercase;
}
.interactive-box .box-desc { margin: 0 0 0.6rem; color: #777; font-size: 12px; }
.slider-row { display: flex; align-items: center; gap: 10px; margin: 0.4rem 0; }
.slider-row label { min-width: 100px; font-size: 12px; color: #555; }
.slider-row input[type="range"] { flex: 1; accent-color: #4a6fa5; }
.slider-row .val { min-width: 50px; text-align: right; font-family: monospace; font-size: 12px; color: #2d3142; font-weight: 600; }
.result-row { margin-top: 0.6rem; padding-top: 0.6rem; border-top: 1px solid #ddd5cc; font-size: 13px; color: #2d3142; }
.result-row .result-val { font-family: monospace; font-weight: 700; color: #4a6fa5; font-size: 14px; }
.boost-table { width: 100%; border-collapse: collapse; margin-top: 0.4rem; font-size: 12px; }
.boost-table th, .boost-table td { padding: 4px 8px; border: 1px solid #ddd5cc; text-align: right; }
.boost-table th { background: #eeecea; font-weight: 600; text-align: center; font-size: 10px; text-transform: uppercase; letter-spacing: 0.05em; }
.boost-table td:first-child { text-align: left; }
canvas.widget-canvas { display: block; margin: 0.5rem auto 0; border: 1px solid #ddd5cc; border-radius: 3px; background: #fff; }
</style>


Direct Preference Optimization (DPO) is a technique for aligning language models with human preferences, without needing reinforcement learning. It replaces the traditional RLHF pipeline with a single supervised fine-tuning step and a clever loss function.

# Overview

Imagine you've built a chatbot that can write text, but it sometimes says unhelpful or weird things. You want to teach it to respond the way humans actually prefer. The question is: how do you do that efficiently?

<div class="interactive-box" id="llm-demo">
<div class="box-title">LLM Response Generation</div>
<div class="box-desc">Pick a prompt and watch the model generate two candidate responses, token by token. A human then picks the preferred one. This is how preference data is collected.</div>
<div style="margin-bottom:0.6rem;">
  <button class="llm-prompt-btn" data-idx="0" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 10px;border:1px solid #4a6fa5;background:#4a6fa5;color:#fff;border-radius:3px;cursor:pointer;margin-right:4px;">Explain gravity</button>
  <button class="llm-prompt-btn" data-idx="1" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 10px;border:1px solid #ddd5cc;background:#fff;color:#4a6fa5;border-radius:3px;cursor:pointer;margin-right:4px;">Write a poem</button>
  <button class="llm-prompt-btn" data-idx="2" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 10px;border:1px solid #ddd5cc;background:#fff;color:#4a6fa5;border-radius:3px;cursor:pointer;">Summarize ML</button>
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

<canvas id="sketch-pipeline" width="780" height="340" style="display:block;margin:1.5rem auto;max-width:100%;border-radius:6px;background:#faf9f7;"></canvas>

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
  <button id="sim-step" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 12px;border:1px solid #4a6fa5;background:#fff;color:#4a6fa5;border-radius:3px;cursor:pointer;margin-right:6px;">Step</button>
  <button id="sim-run" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 12px;border:1px solid #4a6fa5;background:#fff;color:#4a6fa5;border-radius:3px;cursor:pointer;margin-right:6px;">Run 50 steps</button>
  <button id="sim-reset" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 12px;border:1px solid #ccc;background:#fff;color:#888;border-radius:3px;cursor:pointer;">Reset</button>
  <span style="margin-left:10px;font-size:11px;color:#888;">Step: <span id="sim-step-num">0</span></span>
</div>
<canvas id="sim-canvas" width="560" height="200" style="display:block;margin:0.5rem auto 0;border:1px solid #ddd5cc;border-radius:3px;background:#fff;width:100%;"></canvas>
<div style="margin-top:0.5rem;font-size:11px;color:#777;">
  <span style="display:inline-block;width:10px;height:10px;background:#4a6fa5;border-radius:2px;vertical-align:middle;margin-right:3px;"></span> π_θ (current)
  <span style="display:inline-block;width:10px;height:10px;background:#ddd5cc;border-radius:2px;vertical-align:middle;margin-left:12px;margin-right:3px;"></span> π_ref (frozen)
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
<p style="font-weight:600; color:#4a6fa5; margin-top:0.3rem;">The <span class="ke">\beta \log Z(x)</span> is identical in both. It cancels.</p>
</div>

<div class="kd">p^*(y_1 \succ y_2 \mid x) = \sigma\!\left(\beta \log \frac{\pi^*(y_1 \mid x)}{\pi_{ref}(y_1 \mid x)} - \beta \log \frac{\pi^*(y_2 \mid x)}{\pi_{ref}(y_2 \mid x)}\right)</div>

No reward model. No intractable <span class="ke">Z(x)</span>. Just log-ratios of how the policy diverges from the reference.

<canvas id="sketch-cancel" width="780" height="280" style="display:block;margin:1.5rem auto;max-width:100%;border-radius:6px;background:#faf9f7;"></canvas>

# The DPO Loss

<div class="kd">L_{DPO}(\pi_\theta;\, \pi_{ref}) = -\mathbb{E}_{(x,\, y_w,\, y_l)\, \sim\, D}\!\left[\, \log \sigma\!\left(\, \beta \log \frac{\pi_\theta(y_w \mid x)}{\pi_{ref}(y_w \mid x)} - \beta \log \frac{\pi_\theta(y_l \mid x)}{\pi_{ref}(y_l \mid x)} \,\right) \,\right]</div>

<span class="step-line"><span class="step-num">1</span> <span class="ke">\pi_\theta(y_w \mid x) / \pi_{ref}(y_w \mid x)</span>: how much more likely is the winner under the new model vs the original?</span>
<span class="step-line"><span class="step-num">2</span> Same ratio for the loser.</span>
<span class="step-line"><span class="step-num">3</span> Subtract: we want the winner's ratio to exceed the loser's.</span>
<span class="step-line"><span class="step-num">4</span> <span class="ke">\sigma(\cdot)</span>: squash to a probability.</span>
<span class="step-line"><span class="step-num">5</span> <span class="ke">\log</span>: log-likelihood.</span>
<span class="step-line"><span class="step-num">6</span> <span class="ke">-\mathbb{E}</span>: negate and average.</span>

Training pushes the model to increase the probability of winners and decrease the probability of losers, relative to the reference.

<canvas id="sketch-dpo" width="780" height="260" style="display:block;margin:1.5rem auto;max-width:100%;border-radius:6px;background:#faf9f7;"></canvas>

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

<pre style="background:#f7f6f4;border:1px solid #ddd5cc;border-radius:4px;padding:1rem;overflow-x:auto;font-size:0.82rem;line-height:1.55;margin:0.75rem 0;"><code style="font-family:Menlo,Consolas,monospace;color:#2d3142;">import torch
import torch.nn.functional as F

def dpo_loss(pi_lp_w, pi_lp_l, ref_lp_w, ref_lp_l, beta=0.1):
    log_ratio_w = pi_lp_w - ref_lp_w
    log_ratio_l = pi_lp_l - ref_lp_l
    logits = beta * (log_ratio_w - log_ratio_l)
    return -F.logsigmoid(logits).mean()</code></pre>

That's the entire loss. Four lines of math. The rest is plumbing: computing log-probabilities from a language model, loading preference data, and running a training loop.

Below is a runnable version using only NumPy (so it works in the browser). It trains a toy model on preference pairs and prints how the probability distribution shifts. Click the green play button to run it.

<script type="py-editor">
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
  <button class="beta-prompt-btn" data-idx="0" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 10px;border:1px solid #4a6fa5;background:#4a6fa5;color:#fff;border-radius:3px;cursor:pointer;margin-right:4px;">Explain black holes</button>
  <button class="beta-prompt-btn" data-idx="1" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 10px;border:1px solid #ddd5cc;background:#fff;color:#4a6fa5;border-radius:3px;cursor:pointer;margin-right:4px;">Recommend a book</button>
  <button class="beta-prompt-btn" data-idx="2" style="font-family:Inter,sans-serif;font-size:11px;padding:4px 10px;border:1px solid #ddd5cc;background:#fff;color:#4a6fa5;border-radius:3px;cursor:pointer;">Healthy breakfast</button>
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
      <div style="color:#89b4fa;font-size:10px;text-transform:uppercase;letter-spacing:0.1em;margin-bottom:0.3rem;">π_ref (reference model)</div>
      <div id="beta-eff-ref" style="color:#6c7086;min-height:3em;line-height:1.5;font-size:11px;"></div>
    </div>
    <div style="flex:1;min-width:200px;">
      <div style="color:#a6e3a1;font-size:10px;text-transform:uppercase;letter-spacing:0.1em;margin-bottom:0.3rem;">π_θ (DPO-trained at this β)</div>
      <div id="beta-eff-policy" style="color:#cdd6f4;min-height:3em;line-height:1.5;font-size:11px;"></div>
    </div>
  </div>
</div>
<div style="margin-top:0.5rem;display:flex;gap:1rem;flex-wrap:wrap;">
  <div style="flex:1;min-width:120px;background:#f7f6f4;border-radius:4px;padding:0.5rem 0.75rem;text-align:center;">
    <div style="font-size:10px;color:#999;text-transform:uppercase;letter-spacing:0.05em;">KL divergence</div>
    <div id="beta-eff-kl" style="font-family:monospace;font-size:16px;font-weight:700;color:#4a6fa5;margin-top:0.2rem;"></div>
  </div>
  <div style="flex:1;min-width:120px;background:#f7f6f4;border-radius:4px;padding:0.5rem 0.75rem;text-align:center;">
    <div style="font-size:10px;color:#999;text-transform:uppercase;letter-spacing:0.05em;">Reward gain</div>
    <div id="beta-eff-reward" style="font-family:monospace;font-size:16px;font-weight:700;color:#a6e3a1;margin-top:0.2rem;"></div>
  </div>
  <div style="flex:1;min-width:120px;background:#f7f6f4;border-radius:4px;padding:0.5rem 0.75rem;text-align:center;">
    <div style="font-size:10px;color:#999;text-transform:uppercase;letter-spacing:0.05em;">Objective (reward − β·KL)</div>
    <div id="beta-eff-obj" style="font-family:monospace;font-size:16px;font-weight:700;color:#2d3142;margin-top:0.2rem;"></div>
  </div>
</div>
<div id="beta-eff-note" style="font-size:11px;color:#999;margin-top:0.5rem;"></div>
</div>

# Caveats

DPO and RLHF are equivalent in theory, if Bradley-Terry perfectly captures human preferences and you find the global optimum. In practice they can differ: Bradley-Terry is an approximation, the learned reward isn't the true optimal, and gradient descent on a non-convex landscape doesn't find the global optimum. There's also an overfitting risk: if one response *always* wins in the data, DPO pushes the reward gap toward infinity, driving the loser's probability to zero.

<br/>
Feeling generous ? Help me write more blogs like this :)  

<center>
<script type="text/javascript" src="https://cdnjs.buymeacoffee.com/1.0.0/button.prod.min.js" data-name="bmc-button" data-slug="abhinandandubey" data-color="#FFDD00" data-emoji=""  data-font="Cookie" data-text="Buy me a coffee" data-outline-color="#000" data-font-color="#000" data-coffee-color="#fff" ></script>
</center>
<br/>

<script>
document.addEventListener('DOMContentLoaded', function() {
  // Render KaTeX
  document.querySelectorAll('.ke').forEach(function(el) {
    try { katex.render(el.textContent, el, {throwOnError: false, displayMode: false}); } catch(e) {}
  });
  document.querySelectorAll('.kd').forEach(function(el) {
    try { katex.render(el.textContent, el, {throwOnError: false, displayMode: true}); } catch(e) {}
  });
  document.querySelectorAll('script[type^="math/tex"]').forEach(function(el) {
    var display = el.type.indexOf('mode=display') > -1;
    var span = document.createElement(display ? 'div' : 'span');
    try { katex.render(el.textContent, span, {throwOnError: false, displayMode: display}); } catch(e) { span.textContent = el.textContent; }
    el.parentNode.replaceChild(span, el);
  });

  function sigmoid(z){return 1/(1+Math.exp(-z));}

  // ── LLM Response Animation ──
  var llmData = [
    {
      prompt: "Explain gravity to a 5-year-old.",
      respA: "Gravity is like an invisible hug from the Earth. Everything wants to stay close to the ground, just like how you want to stay close to your mom. The bigger something is, the stronger it hugs. That's why when you jump, you always come back down. The Earth is really, really big, so it hugs everything on it.",
      respB: "Gravity is a fundamental force of nature described by Einstein's general theory of relativity as the curvature of spacetime caused by mass-energy equivalence, whereby massive objects create geodesic paths that other objects follow, resulting in what we perceive as gravitational attraction proportional to mass.",
      winner: "A",
      reason: "Human prefers Response A: clear, age-appropriate, uses relatable analogies."
    },
    {
      prompt: "Write a short poem about rain.",
      respA: "Rain falls down,\nWater on ground.\nWet outside today.\nStay inside and play.",
      respB: "The sky lets go of what it held too long,\neach drop a note in an unwritten song.\nThe pavement darkens, mirrors form in streets,\nand somewhere, someone listens to the beats.",
      winner: "B",
      reason: "Human prefers Response B: vivid imagery, emotional resonance, better craft."
    },
    {
      prompt: "Summarize what machine learning is in two sentences.",
      respA: "Machine learning is a branch of AI where computers learn patterns from data instead of being explicitly programmed. Given enough examples, the system figures out rules on its own and can make predictions on new, unseen inputs.",
      respB: "Machine learning is when computers do stuff with data. It's used in a lot of things these days and is pretty cool technology that many companies are investing in heavily.",
      winner: "A",
      reason: "Human prefers Response A: precise, informative, actually explains the concept."
    }
  ];
  var llmIdx = 0, llmTimer = null;
  function llmAnimate(idx) {
    if(llmTimer) { clearInterval(llmTimer); llmTimer = null; }
    llmIdx = idx;
    var d = llmData[idx];
    document.getElementById('llm-prompt').textContent = d.prompt;
    document.getElementById('llm-resp-a').textContent = '';
    document.getElementById('llm-resp-b').textContent = '';
    document.getElementById('llm-verdict').innerHTML = '';
    document.querySelectorAll('.llm-prompt-btn').forEach(function(b,i){
      b.style.background = i===idx ? '#4a6fa5' : '#fff';
      b.style.color = i===idx ? '#fff' : '#4a6fa5';
      b.style.borderColor = i===idx ? '#4a6fa5' : '#ddd5cc';
    });
    var tokA = d.respA.split(/(?<=\s)|(?=\n)/);
    var tokB = d.respB.split(/(?<=\s)|(?=\n)/);
    var maxLen = Math.max(tokA.length, tokB.length);
    var pos = 0;
    var elA = document.getElementById('llm-resp-a');
    var elB = document.getElementById('llm-resp-b');
    llmTimer = setInterval(function(){
      if(pos < tokA.length) elA.textContent += tokA[pos];
      if(pos < tokB.length) elB.textContent += tokB[pos];
      pos++;
      if(pos >= maxLen) {
        clearInterval(llmTimer); llmTimer = null;
        var wColor = d.winner === 'A' ? '#a6e3a1' : '#f38ba8';
        var wLabel = d.winner === 'A' ? 'Response A' : 'Response B';
        document.getElementById('llm-verdict').innerHTML =
          '<span style="color:#4a6fa5;font-weight:600;">Human verdict:</span> ' +
          '<span style="background:' + wColor + '22;color:' + wColor + ';padding:2px 8px;border-radius:3px;font-weight:600;font-size:11px;">' +
          '✓ ' + wLabel + ' wins</span>' +
          '<span style="display:block;margin-top:0.3rem;color:#999;font-size:11px;">' + d.reason + '</span>' +
          '<span style="display:block;margin-top:0.3rem;color:#bbb;font-size:10px;">This (prompt, winner, loser) triple is one training example for DPO.</span>';
      }
    }, 35);
  }
  document.querySelectorAll('.llm-prompt-btn').forEach(function(btn){
    btn.addEventListener('click', function(){ llmAnimate(parseInt(this.dataset.idx)); });
  });
  llmAnimate(0);

  // ── β Effect on Model Output ──
  var betaEffData = [
    {
      prompt: "Explain black holes",
      ref: "A black hole is a region of spacetime where gravity is so strong that nothing, not even light or other electromagnetic waves, has enough energy to escape the event horizon. The theory of general relativity predicts that a sufficiently compact mass can deform spacetime to form a black hole. The boundary of no escape is called the event horizon. A black hole has a great effect on the fate and circumstances of an object crossing it, but has no locally detectable features according to general relativity.",
      responses: [
        "A black hole is a place in space where gravity pulls so hard that even light cannot escape. Because no light gets out, they are invisible. They form when a massive star collapses at the end of its life. Despite their name, they are not empty. They pack enormous mass into a tiny space.",
        "A black hole is where gravity is extremely strong. Light cannot escape from it. They form from collapsed stars. They are very dense.",
        "Black hole = collapsed star, gravity so strong light can't escape. Invisible. Incredibly dense. That's it.",
        "A black hole is a cosmic phenomenon arising from stellar gravitational collapse. When a massive star exhausts its nuclear fuel, the core implodes under its own gravity, compressing matter beyond the Chandrasekhar limit. The resulting singularity warps spacetime so severely that within the Schwarzschild radius, escape velocity exceeds the speed of light. This boundary, the event horizon, represents a causal disconnect from the observable universe. Hawking radiation, a quantum mechanical effect, suggests black holes slowly evaporate over cosmological timescales."
      ],
      rewards: [0.9, 0.5, 0.2, -0.3],
      refIdx: 3
    },
    {
      prompt: "Recommend a book",
      ref: "I would recommend 'Thinking, Fast and Slow' by Daniel Kahneman. It is a comprehensive exploration of the two systems that drive the way we think. System 1 is fast, intuitive, and emotional; System 2 is slower, more deliberative, and more logical. The book draws on decades of research in psychology and behavioral economics to explain how these two systems shape our judgments and decisions. It covers topics including cognitive biases, prospect theory, and the distinction between the experiencing self and the remembering self.",
      responses: [
        "'Thinking, Fast and Slow' by Daniel Kahneman. It explains how we actually make decisions, through two mental systems: one fast and intuitive, one slow and deliberate. Changed how I think about my own thinking. Accessible but deep.",
        "'Thinking, Fast and Slow' by Kahneman. About how the brain makes decisions. Two systems, biases, good read.",
        "Kahneman. Fast/Slow. Read it.",
        "I would suggest considering 'Thinking, Fast and Slow' by the Nobel laureate Daniel Kahneman, published in 2011 by Farrar, Straus and Giroux. This seminal work synthesizes decades of research conducted by Kahneman and his late colleague Amos Tversky. The book systematically examines cognitive biases including anchoring, availability heuristic, representativeness, loss aversion, the endowment effect, and framing effects, all within the theoretical framework of dual-process theory."
      ],
      rewards: [0.9, 0.4, 0.1, -0.2],
      refIdx: 3
    },
    {
      prompt: "Healthy breakfast",
      ref: "For a healthy breakfast, you might consider several options. Oatmeal is an excellent choice as it provides complex carbohydrates and soluble fiber, which can help lower cholesterol levels. You could also consider Greek yogurt, which is high in protein and probiotics that support digestive health. Eggs are another nutritious option, providing high-quality protein and essential nutrients including choline, which is important for brain function. Fresh fruits and vegetables can complement any of these options. Whole grain toast with avocado provides healthy fats and fiber.",
      responses: [
        "Greek yogurt with berries and a handful of nuts. High protein, good fats, antioxidants, and it takes two minutes. Add a drizzle of honey if you want. Overnight oats are great too: oats, milk, chia seeds, fruit, mix the night before.",
        "Greek yogurt with fruit. Oatmeal. Eggs. All healthy options.",
        "Yogurt. Oats. Eggs. Done.",
        "A nutritionally optimal breakfast should incorporate macronutrient balance across proteins, complex carbohydrates, and unsaturated fatty acids. Consider beginning with steel-cut oats (providing beta-glucan soluble fiber at approximately 4g per serving) topped with mixed berries (rich in anthocyanins and ellagic acid), accompanied by a serving of Greek yogurt (supplying approximately 15-20g of casein and whey protein along with Lactobacillus and Bifidobacterium probiotics), and a small portion of mixed nuts (providing monounsaturated and polyunsaturated fatty acids)."
      ],
      rewards: [0.9, 0.4, 0.1, -0.3],
      refIdx: 3
    }
  ];
  var betaEffIdx = 0;
  var betaEffTimer = null;
  function betaEffSelect(idx) {
    betaEffIdx = idx;
    document.querySelectorAll('.beta-prompt-btn').forEach(function(b,i){
      b.style.background = i===idx ? '#4a6fa5' : '#fff';
      b.style.color = i===idx ? '#fff' : '#4a6fa5';
      b.style.borderColor = i===idx ? '#4a6fa5' : '#ddd5cc';
    });
    betaEffUpdate();
  }
  function betaEffUpdate() {
    if(betaEffTimer) { clearInterval(betaEffTimer); betaEffTimer = null; }
    var d = betaEffData[betaEffIdx];
    var raw = parseInt(document.getElementById('beta-eff-slider').value);
    var beta = raw / 100 * 2.0;
    document.getElementById('beta-eff-val').textContent = beta.toFixed(2);
    document.getElementById('beta-eff-prompt').textContent = d.prompt;
    // pick response based on beta: low beta -> idx 2-3 (aggressive), high beta -> idx 3 (ref-like)
    // compute softmax over (reward - beta * "distance") to pick
    var weights = d.rewards.map(function(r, i) {
      var dist = i === d.refIdx ? 0 : Math.abs(i - d.refIdx) * 0.5;
      return Math.exp((r - beta * dist) / Math.max(0.1, 1 - beta * 0.4));
    });
    var sumW = weights.reduce(function(a,b){return a+b;},0);
    var probs = weights.map(function(w){return w/sumW;});
    // pick the response with highest probability
    var bestIdx = 0, bestP = 0;
    for(var i=0;i<probs.length;i++) { if(probs[i]>bestP){bestP=probs[i];bestIdx=i;} }
    // at very high beta, blend toward reference
    var policyText = d.responses[bestIdx];
    if(beta > 1.5) policyText = d.responses[d.refIdx];
    else if(beta > 1.0) {
      // might show the more conservative response
      var conservIdx = Math.min(bestIdx + 1, d.responses.length - 2);
      policyText = d.responses[conservIdx];
    }
    // simple mapping: beta 0-0.3 -> response[2], 0.3-0.7 -> response[1], 0.7-1.2 -> response[0], 1.2+ -> response[3]
    if(beta < 0.2) policyText = d.responses[2];
    else if(beta < 0.5) policyText = d.responses[1];
    else if(beta < 1.2) policyText = d.responses[0];
    else policyText = d.responses[3];
    // compute display metrics
    var rewardVal = beta < 0.2 ? d.rewards[2] : beta < 0.5 ? d.rewards[1] : beta < 1.2 ? d.rewards[0] : d.rewards[3];
    var klVal = beta < 0.2 ? 4.2 : beta < 0.5 ? 2.1 : beta < 1.2 ? 0.8 : 0.05;
    // smooth interpolation
    var t = beta / 2.0;
    klVal = 4.5 * Math.exp(-2.5 * t) + 0.02;
    rewardVal = 0.2 + 0.75 * (1 - t);
    var objVal = rewardVal - beta * klVal;
    document.getElementById('beta-eff-kl').textContent = klVal.toFixed(2);
    document.getElementById('beta-eff-reward').textContent = '+' + rewardVal.toFixed(2);
    document.getElementById('beta-eff-obj').textContent = objVal.toFixed(2);
    // type out the responses
    var refEl = document.getElementById('beta-eff-ref');
    var polEl = document.getElementById('beta-eff-policy');
    refEl.textContent = '';
    polEl.textContent = '';
    var refTok = d.ref.split(/(?<=\s)/);
    var polTok = policyText.split(/(?<=\s)/);
    var maxT = Math.max(refTok.length, polTok.length);
    var pos = 0;
    betaEffTimer = setInterval(function(){
      if(pos < refTok.length) refEl.textContent += refTok[pos];
      if(pos < polTok.length) polEl.textContent += polTok[pos];
      pos++;
      if(pos >= maxT) {
        clearInterval(betaEffTimer); betaEffTimer = null;
      }
    }, 25);
    // note
    var note = '';
    if(beta < 0.2) note = 'Very low β: the model aggressively optimizes for reward, producing terse responses far from the reference. High KL divergence.';
    else if(beta < 0.5) note = 'Low β: the model favors concise answers but still drifts noticeably from the reference style.';
    else if(beta < 1.2) note = 'Moderate β: good balance. The model improves on the reference (more concise, more helpful) without straying too far.';
    else note = 'High β: the model barely changes from the reference. Safe but limited improvement. The KL leash is very tight.';
    document.getElementById('beta-eff-note').textContent = note;
  }
  document.querySelectorAll('.beta-prompt-btn').forEach(function(btn){
    btn.addEventListener('click', function(){ betaEffSelect(parseInt(this.dataset.idx)); });
  });
  var betaSlider = document.getElementById('beta-eff-slider');
  if(betaSlider) {
    betaSlider.addEventListener('input', betaEffUpdate);
    betaEffSelect(0);
  }

  // ── Training Simulator ──
  var responses = [
    {name: '"Concise summary" (winner)', ref: 0.30, logp: Math.log(0.30), isW: true},
    {name: '"Verbose summary" (loser)',   ref: 0.25, logp: Math.log(0.25), isL: true},
    {name: '"Off-topic rant"',            ref: 0.10, logp: Math.log(0.10)},
    {name: '"Refuses to answer"',         ref: 0.15, logp: Math.log(0.15)},
    {name: 'Other responses',             ref: 0.20, logp: Math.log(0.20)}
  ];
  var simStep = 0;
  function simProbs() {
    var exps = responses.map(function(r){return Math.exp(r.logp);});
    var sum = exps.reduce(function(a,b){return a+b;},0);
    return exps.map(function(e){return e/sum;});
  }
  function drawSim() {
    var canvas = document.getElementById('sim-canvas');
    if(!canvas) return;
    var ctx = canvas.getContext('2d');
    var W = canvas.width, H = canvas.height;
    ctx.clearRect(0,0,W,H);
    var probs = simProbs();
    var n = responses.length;
    var barW = 50, gap = (W - n*barW*2 - 40) / (n-1);
    var startX = 20, maxH = H - 40;
    for(var i=0;i<n;i++){
      var x = startX + i*(barW*2 + gap);
      var refH = responses[i].ref * maxH / 0.6;
      var curH = probs[i] * maxH / 0.6;
      // ref bar
      ctx.fillStyle='#ddd5cc';
      ctx.fillRect(x, H-20-refH, barW-2, refH);
      // current bar
      var col = responses[i].isW ? '#4a6fa5' : responses[i].isL ? '#c0392b' : '#8e94a8';
      ctx.fillStyle=col;
      ctx.fillRect(x+barW, H-20-curH, barW-2, curH);
      // label
      ctx.fillStyle='#555';ctx.font='9px Inter,sans-serif';ctx.textAlign='center';
      var label = responses[i].name.length > 16 ? responses[i].name.substring(0,15)+'...' : responses[i].name;
      ctx.fillText(label, x+barW-1, H-6);
      // prob values
      ctx.fillStyle='#999';ctx.font='10px monospace';
      ctx.fillText((probs[i]*100).toFixed(1)+'%', x+barW-1, H-22-Math.max(refH,curH));
    }
  }
  function simLoss() {
    var probs = simProbs();
    var beta = parseFloat(document.getElementById('sim-beta').value);
    var wIdx=0, lIdx=1;
    var logRatioW = Math.log(probs[wIdx]) - Math.log(responses[wIdx].ref);
    var logRatioL = Math.log(probs[lIdx]) - Math.log(responses[lIdx].ref);
    var inside = beta * (logRatioW - logRatioL);
    var pref = sigmoid(inside);
    var loss = -Math.log(pref);
    return {loss:loss, pref:pref, logRatioW:logRatioW, logRatioL:logRatioL};
  }
  function simGradStep() {
    var beta = parseFloat(document.getElementById('sim-beta').value);
    var lr = parseFloat(document.getElementById('sim-lr').value);
    var probs = simProbs();
    var info = simLoss();
    var gradScale = (1 - info.pref) * beta;
    // winner: increase log-prob
    responses[0].logp += lr * gradScale;
    // loser: decrease log-prob
    responses[1].logp -= lr * gradScale;
    simStep++;
    updateSimDisplay();
  }
  function updateSimDisplay() {
    var info = simLoss();
    document.getElementById('sim-loss').textContent = info.loss.toFixed(4);
    document.getElementById('sim-pref').textContent = info.pref.toFixed(4);
    document.getElementById('sim-step-num').textContent = simStep;
    drawSim();
  }
  function simReset() {
    responses[0].logp = Math.log(0.30);
    responses[1].logp = Math.log(0.25);
    responses[2].logp = Math.log(0.10);
    responses[3].logp = Math.log(0.15);
    responses[4].logp = Math.log(0.20);
    simStep = 0;
    updateSimDisplay();
  }
  var simBeta = document.getElementById('sim-beta');
  var simLr = document.getElementById('sim-lr');
  if(simBeta) {
    simBeta.addEventListener('input', function(){document.getElementById('sim-beta-val').textContent=parseFloat(this.value).toFixed(1);updateSimDisplay();});
    simLr.addEventListener('input', function(){document.getElementById('sim-lr-val').textContent=parseFloat(this.value).toFixed(2);});
    document.getElementById('sim-step').addEventListener('click', simGradStep);
    document.getElementById('sim-run').addEventListener('click', function(){for(var i=0;i<50;i++)simGradStep();});
    document.getElementById('sim-reset').addEventListener('click', simReset);
    updateSimDisplay();
  }

  // ── Boost factor table ──
  var bB=document.getElementById('boost-beta');
  var R=[{ref:.3,rw:2.5,b:'b1',p:'p1'},{ref:.4,rw:2,b:'b2',p:'p2'},{ref:.001,rw:-1,b:'b3',p:'p3'}];
  function updateB(){
    var beta=parseFloat(bB.value);document.getElementById('boost-beta-val').textContent=beta.toFixed(1);
    var pr=[];R.forEach(function(r){var b=Math.exp(r.rw/beta);pr.push(r.ref*b);document.getElementById(r.b).textContent=b.toFixed(3);});
    var Z=pr.reduce(function(a,b){return a+b;},0);
    R.forEach(function(r,i){document.getElementById(r.p).textContent=(pr[i]/Z).toFixed(4);});
    document.getElementById('boost-z').textContent=Z.toFixed(4);
  }
  if(bB){bB.addEventListener('input',updateB);updateB();}

  // ── DPO loss playground ──
  var dB=document.getElementById('dpo-beta'),dW=document.getElementById('dpo-pw'),dL=document.getElementById('dpo-pl');
  function updateD(){
    var beta=parseFloat(dB.value),pw=parseFloat(dW.value),pl=parseFloat(dL.value);
    document.getElementById('dpo-beta-val').textContent=beta.toFixed(2);
    document.getElementById('dpo-pw-val').textContent=pw.toFixed(2);
    document.getElementById('dpo-pl-val').textContent=pl.toFixed(2);
    var ins=beta*(Math.log(pw/.2)-Math.log(pl/.25)),pref=sigmoid(ins),loss=-Math.log(pref);
    document.getElementById('dpo-pref').textContent=pref.toFixed(4);
    document.getElementById('dpo-loss').textContent=loss.toFixed(4);
    var h=document.getElementById('dpo-hint');
    if(pref>.8)h.textContent='Strongly prefers the winner. Loss is low.';
    else if(pref>.5)h.textContent='Leans toward the winner, not strongly.';
    else if(pref>.3)h.textContent='Nearly indifferent.';
    else h.textContent='Prefers the loser. Loss is high.';
  }
  if(dB){dB.addEventListener('input',updateD);dW.addEventListener('input',updateD);dL.addEventListener('input',updateD);updateD();}

  // ── Hand-drawn sketches ──
  var skFont = '19px Patrick Hand, sans-serif';
  var skFontSm = '15px Patrick Hand, sans-serif';
  var skFontLg = '23px Patrick Hand, sans-serif';
  var skBlue = '#4a6fa5';
  var skDark = '#2d3142';
  var skGray = '#999';
  var skRed = '#c0392b';
  var skGreen = '#27ae60';

  function wobbleLine(ctx, x1, y1, x2, y2) {
    var dx = x2-x1, dy = y2-y1, len = Math.sqrt(dx*dx+dy*dy);
    var steps = Math.max(3, Math.floor(len/20));
    ctx.beginPath();
    ctx.moveTo(x1, y1);
    for(var i=1;i<=steps;i++){
      var t=i/steps;
      var jx = (Math.random()-0.5)*3;
      var jy = (Math.random()-0.5)*3;
      ctx.lineTo(x1+dx*t+jx, y1+dy*t+jy);
    }
    ctx.stroke();
  }

  function wobbleRect(ctx, x, y, w, h) {
    wobbleLine(ctx, x, y, x+w, y);
    wobbleLine(ctx, x+w, y, x+w, y+h);
    wobbleLine(ctx, x+w, y+h, x, y+h);
    wobbleLine(ctx, x, y+h, x, y);
  }

  function wobbleArrow(ctx, x1, y1, x2, y2) {
    wobbleLine(ctx, x1, y1, x2, y2);
    var angle = Math.atan2(y2-y1, x2-x1);
    var hl = 10;
    ctx.beginPath();
    ctx.moveTo(x2, y2);
    ctx.lineTo(x2-hl*Math.cos(angle-0.4), y2-hl*Math.sin(angle-0.4));
    ctx.moveTo(x2, y2);
    ctx.lineTo(x2-hl*Math.cos(angle+0.4), y2-hl*Math.sin(angle+0.4));
    ctx.stroke();
  }

  function wobbleCircle(ctx, cx, cy, r) {
    ctx.beginPath();
    for(var a=0;a<=Math.PI*2+0.1;a+=0.15){
      var jr = r + (Math.random()-0.5)*2;
      var px = cx + jr*Math.cos(a);
      var py = cy + jr*Math.sin(a);
      if(a===0) ctx.moveTo(px,py); else ctx.lineTo(px,py);
    }
    ctx.stroke();
  }

  // Sketch 1: RLHF vs DPO pipeline
  function drawAllSketches() {
  (function(){
    var c = document.getElementById('sketch-pipeline');
    if(!c) return;
    var ctx = c.getContext('2d');
    ctx.lineWidth = 1.5;
    ctx.lineCap = 'round';

    // title
    ctx.font = skFontLg; ctx.fillStyle = skBlue;
    ctx.fillText('The old way (RLHF):', 20, 35);

    // RLHF boxes
    var boxes = [
      {x:20,y:50,w:130,h:42,t:'Human prefs',sub:'(A > B pairs)'},
      {x:190,y:50,w:140,h:42,t:'Reward Model',sub:'train r_φ'},
      {x:370,y:50,w:110,h:42,t:'PPO / RL',sub:'(expensive!)'},
      {x:520,y:50,w:130,h:42,t:'Aligned LLM',sub:'π_θ'}
    ];
    ctx.strokeStyle = skDark;
    boxes.forEach(function(b){
      wobbleRect(ctx, b.x, b.y, b.w, b.h);
      ctx.font = skFont; ctx.fillStyle = skDark;
      ctx.fillText(b.t, b.x+10, b.y+22);
      ctx.font = skFontSm; ctx.fillStyle = skGray;
      ctx.fillText(b.sub, b.x+10, b.y+38);
    });
    // arrows
    ctx.strokeStyle = skBlue;
    for(var i=0;i<boxes.length-1;i++){
      wobbleArrow(ctx, boxes[i].x+boxes[i].w+4, boxes[i].y+21, boxes[i+1].x-4, boxes[i+1].y+21);
    }
    // cross it out
    ctx.strokeStyle = skRed; ctx.lineWidth = 2;
    ctx.globalAlpha = 0.4;
    wobbleLine(ctx, 15, 45, 660, 95);
    wobbleLine(ctx, 15, 95, 660, 45);
    ctx.globalAlpha = 1; ctx.lineWidth = 1.5;

    // DPO
    ctx.font = skFontLg; ctx.fillStyle = skGreen;
    ctx.fillText('The DPO way:', 20, 145);

    var dpoBoxes = [
      {x:20,y:160,w:130,h:42,t:'Human prefs',sub:'(same data!)'},
      {x:220,y:160,w:200,h:42,t:'DPO loss (supervised)',sub:'one step, no RL'},
      {x:480,y:160,w:130,h:42,t:'Aligned LLM',sub:'π_θ'}
    ];
    ctx.strokeStyle = skDark;
    dpoBoxes.forEach(function(b){
      wobbleRect(ctx, b.x, b.y, b.w, b.h);
      ctx.font = skFont; ctx.fillStyle = skDark;
      ctx.fillText(b.t, b.x+10, b.y+22);
      ctx.font = skFontSm; ctx.fillStyle = skGray;
      ctx.fillText(b.sub, b.x+10, b.y+38);
    });
    ctx.strokeStyle = skGreen;
    wobbleArrow(ctx, 154, 181, 216, 181);
    wobbleArrow(ctx, 424, 181, 476, 181);

    // annotation
    ctx.font = skFont; ctx.fillStyle = skBlue;
    ctx.save(); ctx.translate(680, 120); ctx.rotate(-0.15);
    ctx.fillText('skip the', 0, 0);
    ctx.fillText('middleman!', 0, 24);
    ctx.restore();

    // curly brace scribble pointing to removed parts
    ctx.strokeStyle = skRed; ctx.lineWidth = 1;
    ctx.font = skFontSm; ctx.fillStyle = skRed;
    ctx.fillText('← no reward model needed', 190, 115);
    ctx.fillText('← no RL needed', 370, 115);

    // bottom note
    ctx.font = skFontSm; ctx.fillStyle = skGray;
    ctx.fillText('* same preference data in, aligned model out. just fewer steps.', 20, 230);

    // small doodle: smiley
    ctx.strokeStyle = skGreen; ctx.lineWidth = 1.5;
    wobbleCircle(ctx, 700, 180, 16);
    ctx.beginPath(); ctx.arc(694,176,2,0,Math.PI*2); ctx.fill();
    ctx.beginPath(); ctx.arc(706,176,2,0,Math.PI*2); ctx.fill();
    ctx.beginPath(); ctx.arc(700,184,7,0.1,Math.PI-0.1); ctx.stroke();
  })();

  // Sketch 2: The cancellation trick
  (function(){
    var c = document.getElementById('sketch-cancel');
    if(!c) return;
    var ctx = c.getContext('2d');
    ctx.lineWidth = 1.5; ctx.lineCap = 'round';

    ctx.font = skFontLg; ctx.fillStyle = skBlue;
    ctx.fillText('The Z(x) cancellation:', 20, 30);

    // reward expressions
    ctx.font = skFont; ctx.fillStyle = skDark;
    ctx.fillText('r(x, y₁) = β log(π*/π_ref) + β log Z(x)', 40, 70);
    ctx.fillText('r(x, y₂) = β log(π*/π_ref) + β log Z(x)', 40, 100);

    // minus sign
    ctx.font = skFontLg; ctx.fillStyle = skBlue;
    ctx.fillText('subtract:', 40, 135);

    // strike through Z(x) parts
    ctx.strokeStyle = skRed; ctx.lineWidth = 2.5;
    // first line Z(x)
    wobbleLine(ctx, 370, 62, 530, 62);
    wobbleLine(ctx, 370, 75, 530, 75);
    // second line Z(x)
    wobbleLine(ctx, 370, 92, 530, 92);
    wobbleLine(ctx, 370, 105, 530, 105);

    ctx.lineWidth = 1.5;
    ctx.font = skFont; ctx.fillStyle = skDark;
    ctx.fillText('r(y₁) - r(y₂) = β log(π*(y₁)/π_ref(y₁)) - β log(π*(y₂)/π_ref(y₂))', 40, 170);

    // annotation
    ctx.font = skFont; ctx.fillStyle = skRed;
    ctx.save(); ctx.translate(560, 70); ctx.rotate(-0.1);
    ctx.fillText('these cancel!', 0, 0);
    ctx.restore();

    // arrow pointing to result
    ctx.strokeStyle = skGreen; ctx.lineWidth = 1.5;
    wobbleArrow(ctx, 30, 145, 30, 160);

    ctx.font = skFont; ctx.fillStyle = skGreen;
    ctx.save(); ctx.translate(560, 165); ctx.rotate(0.05);
    ctx.fillText('no Z(x)!', 0, 0);
    ctx.fillText('no reward model!', 0, 24);
    ctx.restore();

    // bottom note with lightbulb doodle
    ctx.font = skFontSm; ctx.fillStyle = skGray;
    ctx.fillText('* the intractable partition function disappears because Bradley-Terry only uses reward differences', 40, 230);

    // lightbulb doodle
    ctx.strokeStyle = '#f0c040'; ctx.lineWidth = 1.5;
    wobbleCircle(ctx, 20, 225, 10);
    wobbleLine(ctx, 16, 236, 24, 236);
    wobbleLine(ctx, 17, 240, 23, 240);
    // rays
    wobbleLine(ctx, 20, 212, 20, 206);
    wobbleLine(ctx, 10, 216, 6, 212);
    wobbleLine(ctx, 30, 216, 34, 212);
  })();

  // Sketch 3: What DPO does
  (function(){
    var c = document.getElementById('sketch-dpo');
    if(!c) return;
    var ctx = c.getContext('2d');
    ctx.lineWidth = 1.5; ctx.lineCap = 'round';

    ctx.font = skFontLg; ctx.fillStyle = skBlue;
    ctx.fillText('What DPO training does:', 20, 30);

    // winner side
    ctx.strokeStyle = skGreen;
    wobbleRect(ctx, 40, 50, 300, 70);
    ctx.font = skFont; ctx.fillStyle = skGreen;
    ctx.fillText('y_w (winner)', 55, 75);
    ctx.font = skFontSm; ctx.fillStyle = skDark;
    ctx.fillText('"Gravity pulls objects toward each other"', 55, 100);

    // arrow up
    ctx.strokeStyle = skGreen; ctx.lineWidth = 2;
    wobbleArrow(ctx, 190, 130, 190, 145);
    ctx.font = skFont; ctx.fillStyle = skGreen;
    ctx.fillText('↑ boost probability', 210, 148);
    ctx.lineWidth = 1.5;

    // loser side
    ctx.strokeStyle = skRed;
    wobbleRect(ctx, 420, 50, 300, 70);
    ctx.font = skFont; ctx.fillStyle = skRed;
    ctx.fillText('y_l (loser)', 435, 75);
    ctx.font = skFontSm; ctx.fillStyle = skDark;
    ctx.fillText('"Stuff falls down cuz earth is big"', 435, 100);

    // arrow down
    ctx.strokeStyle = skRed; ctx.lineWidth = 2;
    wobbleArrow(ctx, 570, 130, 570, 145);
    ctx.font = skFont; ctx.fillStyle = skRed;
    ctx.fillText('↓ reduce probability', 590, 148);
    ctx.lineWidth = 1.5;

    // leash annotation
    ctx.strokeStyle = skBlue; ctx.lineWidth = 1.5;
    ctx.setLineDash([4,4]);
    wobbleLine(ctx, 190, 170, 570, 170);
    ctx.setLineDash([]);

    // spring/coil in the middle of the leash
    ctx.strokeStyle = skBlue; ctx.lineWidth = 1.5;
    ctx.beginPath();
    var springX = 330, springW = 100, coils = 6;
    ctx.moveTo(springX, 170);
    for(var i=0;i<coils;i++){
      var sx = springX + (i+0.5)*springW/coils;
      var sy = 170 + (i%2===0 ? -8 : 8);
      ctx.lineTo(sx, sy);
    }
    ctx.lineTo(springX+springW, 170);
    ctx.stroke();

    ctx.font = skFont; ctx.fillStyle = skBlue;
    ctx.fillText('β controls how far π_θ can drift from π_ref', 220, 200);

    // label the spring
    ctx.font = skFontSm; ctx.fillStyle = skGray;
    ctx.fillText('← KL leash (β) →', 340, 160);

    // bottom
    ctx.font = skFontSm; ctx.fillStyle = skGray;
    ctx.fillText('* all relative to the frozen reference model. no separate reward model involved.', 40, 245);
  })();
  } // end drawAllSketches
  // Use FontFace API to explicitly load the font for canvas rendering
  var pf = new FontFace('Patrick Hand', "url('https://fonts.gstatic.com/s/patrickhand/v25/LDI1apSQOAYtSuYWp8ZhfYe8XsLLubg58w.woff2')");
  pf.load().then(function(loaded) {
    document.fonts.add(loaded);
    drawAllSketches();
  }).catch(function() {
    // fallback: try drawing anyway after a delay
    setTimeout(drawAllSketches, 1000);
  });
});
</script>