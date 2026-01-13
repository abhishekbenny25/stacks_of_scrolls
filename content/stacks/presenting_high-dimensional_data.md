---
date: '2026-01-13T13:48:40+04:00'
draft: false
title: 'Presenting High-Dimensional Data (Without Summoning Eldritch Horrors🦑)'

tags: []
author: Abhishek Benny

showToc: true
TocOpen: false
hidemeta: false
comments: false
description: "A guide on presenting complex high-dimensional data to executive audiences"


hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true

cover:
  image: images/covers/presenting_high-dimensional_data.png
  alt: "Spawning Eldritch horror from unclear data in presentation"
  caption: ""
  relative: true
  responsiveImages: true
---

Presenting high dimensional data to executives isn’t really about data, it’s about steering the decision. Approve capital, change a process, cull an underperforming product. In the wonderland of our imaginations, there exists a two variable system, where the decision is cut and dry – but this is never the case with real data. Data out in the wild always comes with dozens if not hundreds of variables, from complex KPIs to individual sensor data and if you try to show all of it at once, you can expect to open a portal to confusion in PowerPoint form. 

This is how we spawn eldritch horrors, not by doing bad analysis, but by cognitively overloading the target audience. An overwhelmed audience will either tune out, latch onto a random outlier, or argue about definitions and “faults” rather than making the decision you are trying to drive. 

Now, to not summon eldritch horrors, a quick rule of thumb: If your core visual needs more than 3 axes to make the point, you have got to reduce it down. The goal is not to remove information or obfuscate marginally good data, it is to compress the complexity into a smaller number of dimensions that preserve the story of the data while maintaining support for your message - keeping the room oriented towards taking an action.

In this article, I will walk through a workflow for taming high-dimensional data for executive audiences. 

## Step 0 – Know your audience

Before you look at the data, clarify to yourself what the audience wants to walk away with. 

If they’re senior leaders far from the technical details, keep the narrative high level and decision first. These leaders are usually only looking for what’s happening, why it matters, what your recommendations are and how much they cost. The data narrative should be kept minimal. If they’re closer to the work, include more details and data – however avoid dumping highly complex data. 

An overwhelmed audience can misinterpret what you show them, which is the exact opposite of what you want. Leadership will aggressively shut down any ideas they deem problematic in the future, so **DO NOT** allow them to misinterpret your data. 

Also make sure you account for the room-dynamics. Different executives optimize for different objectives (risk, workload, speed, reputation). Sometime it is better to frame your analysis and the decision in a way that matches the objectives of the room rather than your “ideal” framing.

## Step 1 – Scope the data and eliminate “lurkers”
To influence a decision, your visuals need to do two things:
1)	Support your thesis
2)	Show evidence of benefit or risk to the organization
Everything you visualize MUST satisfy these objectives. The biggest enemies to a clear decision are confusion and distraction – usually introduced through “lurkers”.

“Lurkers” are data that are present in your analysis, but does not materially strengthen or weaken your main thesis. For example:
-	Data that only matters under a low-probability condition 
-	Data that varies, but does not change the recommendation
-	Garbage metrics: Vanity metrics, duplicate metrics, or inconsistently defined metrics.

**You do not need to present something just because you measured it.**

At this point, if you are already down to one or two variables, you’re basically done! Keep it simple and focus on the decision.

## Step 2 – Collapse correlated variables into a smaller set of axes
If you’ve kept only the most relevant data and you still have more than 3 dimensions, the next step is to combine variables that move together.

In practice, this is usually done in the form of dimension reduction – where you identify correlated variables and replace them with a smaller set of “embedded” axes while still capturing the majority of what they show.

One of the common approaches to reduce dimensionality is Principal Component Analysis (PCA) – which can be simplified down to these steps:

1)	Normalize your metrics: this is done so that scale variances in the metrics do not dominate the eigenvalues. Usually done with the minimum at 0 and max at 1 (or % increases in time series). 
2)	Compute the covariance matrix: which shows the variances between your variables in a neat triangle matrix.
3)	Find the eigenvalues and eigenvectors of the covariance matrix to get the principal components. 

The result of this method is a list of eigenvalue/vector pairs, of which the higher the eigenvalue, the more important the vector. You can then decide how many vectors to present – a common heuristic here is to look for eigenvalues that are greater than 0.2 when divided by the sum of eigenvalues (make up 20% of the pattern). 

Now if you just present this as eigenvalues, you’re going to lose your audience. We need to explain to the audience that many of the metrics moved together, or that the data has been compressed into themes. This may raise questions of how metrics were grouped and how to interpret these new groupings, for which we should show a table of contributing variables.

**Table 1.   Main themes and their top contributing variables**


|     Category Name    | |    Variables    |
|---|---|---|
|     Operational   Strain <br> (Lower is better)    ||     Cycle time <br>    Rework   <br>  Defect rate    |
|     Pricing   Factors <br>(Higher is better)    ||     Average sale   price <br>     Cost of goods   produced  <br>   Win rate at   list price    |



This converts “eigenvectors” into something executives can easily reason on and move to action. 

### Words of caution
Even with very careful filtering you’ll occasionally end up with a group that looks nonsensical. This is not an artifact; this is a feature – the data is telling you something. Drill down in the data and find a reason as you are sure to be asked about it. It also helps building trust with the audience if you show you have already thoroughly interrogated the data and unpacked what outliers mean. 

To handle this properly, prepare drill-down visuals in an appendix: these should be small, focused charts that unpack the drivers for trends observed in the main visual. These can be verbose and include all your data – but to reduce work on your end, anticipate the questions that may be asked and prepare visuals that explain them. Keep this ready for Q&A, **DO NOT PRESENT THEM UNLESS ASKED**. Your primary presentation should remain clean and decision oriented – the appendix only exists to defend the main point when needed. 

## Closing the portal

Visualizing high-dimensional data for executives isn’t about showing them everything your wide business intelligence system knows, it is showing them exactly what they need to make a decision  - with just enough evidence for them to be confident in it. By following this guide, you will have anchored your narrative to a business decision, cut out any metrics that don’t materially change your message, and then compress what is left into a few easy to explore themes that fit comfortably into a few easily interpretable slides. Keep your presentation concise, giving context to the problem, then providing a recommendation with data evidence, and finally bringing a call to action. Maintain an appendix to defend your data and answer any mechanical questions without derailing your presentation. If you do this well, you don’t just avoid spawning eldritch horrors, you turn multidimensional complexity into clear, actionable recommendations. 

**And thus, no monsters were summoned.**

<small> **genAI declaration:** The text matter of this blog post was completed without the use of generative AI tools. Cover image created using OpenAI’s image generation tool (DALL·E). Prompt: Corporate meeting room, projector screen with a 3D scatter plot filled with multicolored random points. Tentacles emerging out of the projected image into the room. Presenter and audience reacting. Clean composition, blog cover illustration, landscape. - Abhishek Benny.</small>