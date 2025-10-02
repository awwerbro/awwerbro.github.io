---
layout: post
date: 2025-10-01
inline: false
related_posts: false
tags: agents, ALD, autonomous science
title: LLM Agents for knowledge discovery in ALD
image: spaces_with_ill.png
summary: "LLM-based agents discovered ALD, ALE and area-selective deposition protocols in a realistic simulator by open-ended experimentation."
---

<div class="row justify-content-center">
    <div class="col-10 col-md-6 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/spaces_with_ill.png" title="input-rule-output space of the system under study" class="img-fluid rounded z-depth-1" style="max-width: 100%; height: auto;" %}
    </div>
</div>
<div class="row justify-content-center">
    <div class="col-10 col-md-6">
        <div class="caption text-center" style="font-size: 0.95em;">
            <strong>Figure:</strong> Input-rule-output space of the system under study. Instead of asking the agent to answer a question using the tool (blue line) we asked it to figure out the rules of the tool by experimenting with it.    </div>
    </div>
</div>

**The alien market**

After a long trip through hyperspace, you took the wrong intergalactic exit and now your spaceship has crashed on an unknown alien planet. However, you're not completely out of luck today. There is a market on the planet where you can buy anything you can dream of. Anything? No, it turns out there are certain, specific rules to buying things there, and you better figure out these rules quickly! You can ask the aliens if they sell a certain object, and the aliens will answer with 'yes' or 'no' to indicate whether they sell it or not. Could you figure out these rules? 

We used to play this game as kids, and we called it 'the market of padipader' (*pas d'i, pas d'er*). Recently we played it again, but this time as a toy example to study if an LLM-based agent can *discover* knowledge by interrogating a system (or *tool*) we give it. The LLM agent can call the market as many times as it wants, until it thinks it found out the rules. This is not easy, even for humans (as we discovered during our group meeting). 

More specifically, the challenge is that we did **not** ask the LLM to answer a specific question, such as *"can I buy a rocket motor at the market?"* after which it would call the tool with "rocket motor" and give us back "yes" or "no". This would be the blue line in the graph above and is a pretty common use case of tools (and something most benchmarks are probing). Instead, we do not ask the agent anything except to figure out the rules of the market by doing experiments (or, more abstract: figuring out what the orange lines in the figure are). This is a lot harder, and it turns out that the LLM agents (often) manage to figure out the rules without additional input, if we just push them hard enough to not give up. This was a cool result, but we weren't done yet.

**Discovering atomic layer processing**

After the toy example, we gave the agents another tool: a simulated purge-type atomic layer deposition (ALD) reactor with 4 different, fictitious chemicals (so the agent does not rely on prior chemical knowledge). In situ sensors provide the agent with a limited window to probe what is happening inside (for the connoisseurs: a pressure sensor and a quartz crystal microbalance). The tool is pretty realistic, uses the same recipe format as our actual physical reactor and simulates mass transport, gas-surface processes, temperature-dependent reaction kinetics and decomposition reactions (we were having fun...). We explicitly hid specific gas-surface reactions in the simulation, such that when the chemicals are combined in the right way, different modes of atomic layer deposition (ALD), atomic layer etching (ALE) and even blocking/area-selective deposition (ASD) (a set of tools also known as atomic layer processing) are possible. 

However, here's the catch. Similar to the alien market, we did not explicitly instruct the agent to develop ALD/ALE/ASD - and we even deliberately avoided using specific terminology.  We just gave the simulation to the agent with instructions on how to change gas flows, open/close valves and change temperatures. So instead of asking "what happens if you do a 1s pulse A - 10s purge A - 1s pulse B - 10s purge B?" (again, blue line in the figure above), we asked it to find out how to use the reactor and generate meaningful statements about it (orange lines in the figure above). As such, the agent tried many different strategies that did or did not work: combining chemicals that didn't react, co-dosing chemicals (CVD), intentionally decomposing gas on surfaces, starting from the same surface for different experiments, probing the temperature dependence of reactions, but all of that autonomously and *without being prompted to do so*... 

In research, there truly is no *right* answer, and also failed experiments taught the LLM something (figuring out which objects one can not buy at the alien market is also important!). And indeed, in the end, given sufficient time, the agent was able to figure out how to do ALD, ALE, and ASD with the simulated reactor, and provided some pretty detailed summaries about its findings.

**Conclusions**

The lesson learned here is that in some cases, the key to true discovery is to not limit agents by defining a specific objective (something we are very prone to as scientists), but letting the agent freely experiment with the system and follow interesting leads (within soft safety bounds). 

There are some philosophical parallels between this work and scientific research in general: the importance of failing and persistence to truly understand the system you study, the path-dependence of results and insights, and the fact that research is never truly done: some undiscovered knowledge may always be lurking in an unexplored corner of the system.

There's much more to this, and we hope to push this a lot further in the future/make all code publicly available (coming soon)! In the meantime, you can read the specifics of it in [our accepted conference paper here](https://arxiv.org/abs/2509.26201)

Finally, many thanks to the co-authors Marshall B. Lindsay, Matthew Maschmann and Matthias J. Young! Comments and questions welcome!