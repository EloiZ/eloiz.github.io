---
layout: post
date: 2026-08-07 07:59:00-0400
inline: true
related_posts: false
---

New preprint: [Pictura](/publications#yin2026pictura) is a GPU-accelerated multi-agent driving simulator that renders every agent's egocentric view at each step, sustaining up to 500K agent-steps/s on a single H100. With it, we train Alberti by self-play with plain PPO over 50B agent steps (~35M km): the first large-scale driving self-play policy learned directly from perspective images, with no privileged observation of the surroundings.
