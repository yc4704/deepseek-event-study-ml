# LDA Topic Labels

This document lists the human-curated labels for the 12 topics learned by
the LDA model trained on 400 firm-level earnings-call documents.

**Model**: `sklearn.decomposition.LatentDirichletAllocation`
**Number of topics (K)**: 12
**Perplexity**: 1976.36
**Selection criterion**: K chosen by combining perplexity trajectory
(flat minimum across K=8-15) with qualitative interpretability of top-15
words per topic. K=12 yielded the cleanest separation between AI-related
and non-AI-related narratives.

## AI-related topics

Topics **flagged as AI-related** (based on presence of AI seed vocabulary
and qualitative analysis of AI narrative in top words):

- Topics: [2, 3, 8, 9, 11]

These are summed into `ai_topic_loading_sum` as a single aggregate measure
of AI narrative intensity per firm.

## Topic dictionary

For each topic: label, AI-flag, and the top 15 most representative words.

### Topic 0: Energy & Utilities 

Top 15 words: `power, energy, battery, project, grid, europe, storage, plant, gas, system, data, center, commercial, scale, utility`

### Topic 1: Advertising & Marketing 

Top 15 words: `brand, ad, user, advertiser, people, data, marketing, content, medium, advertising, consumer, ctv, spend, video, store`

### Topic 2: AI Semiconductor Design **[AI-related]**

Top 15 words: `design, inventory, power, ai, data, center, industrial, sequentially, application, automotive, silicon, mix, ramp, portfolio, billion`

### Topic 3: AI Enterprise & Healthcare Services **[AI-related]**

Top 15 words: `client, data, ai, deal, digital, billion, area, capability, health, acquisition, model, adjusted, top, feel, relationship`

### Topic 4: Financial Reporting (Geographic) 

Top 15 words: `adjusted, inventory, segment, acquisition, order, ebitda, america, europe, lower, eur, actually, course, digit, north, profit`

### Topic 5: Financial Reporting (Accounting) 

Top 15 words: `adjusted, ebitda, prior, program, experience, payment, marketing, offering, capital, fourth, free, bank, benefit, driving, seen`

### Topic 6: Government & Defense 

Top 15 words: `system, contract, people, actually, government, application, quantum, software, process, network, done, state, world, talk, excited`

### Topic 7: Project Execution & Capital 

Top 15 words: `program, project, slide, order, capital, fourth, backlog, segment, manufacturing, adjusted, solar, production, facility, defense, strategic`

### Topic 8: China AI Semiconductor Supply Chain **[AI-related]**

Top 15 words: `billion, ai, china, production, leading, automotive, advanced, hbm, capacity, volume, edge, order, percentage, supply, dram`

### Topic 9: AI Cloud Infrastructure **[AI-related]**

Top 15 words: `ai, cloud, billion, data, infrastructure, model, software, enterprise, application, storage, fy, capability, scale, workload, nvidia`

### Topic 10: Chinese Gaming & Streaming 

Top 15 words: `user, game, content, rmb, live, gaming, experience, marketing, event, language, foreign, player, billion, streaming, operation`

### Topic 11: AI SaaS & Cybersecurity **[AI-related]**

Top 15 words: `ai, data, enterprise, deal, security, arr, cloud, partner, capability, innovation, model, subscription, saas, momentum, organization`
