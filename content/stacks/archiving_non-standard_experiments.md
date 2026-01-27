---
date: '2026-01-27T11:48:40+04:00'
draft: false
title: 'Archiving Non-Standard Experiments (Entropy-Moth Proofing)'

tags: []
author: Abhishek Benny

showToc: true
TocOpen: false
hidemeta: false
comments: false
description: "A practical guide to building a knowledgebase that stands the test of time"

hideSummary: false
searchHidden: true
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true

cover:
  image: images/covers/archiving_non-standard_experiments.png
  alt: Entropy moths munching on unorganized research data
  caption: ""
  relative: true
  responsiveImages: true
---

In any R&D or continuous improvement environment, progress depends on knowledge transfer. Often, a lot of the most valuable lessons live in people’s heads: the “we tried that in 2014 …” experiments, the one-off protocols, the messy edge cases and details that did not make it into a formal report. Newer teams can try to survey the literature and internal documentation, but without a guide, it’s hard to know what exists, let alone where it’s stored, what to trust, and under what circumstances the data is valid. 

When one of these human libraries leaves, the organization does not just lose their experience, it also loses the retrievability of the data. You’re stranded, repeating work, missing prior constraints, losing context and spending large amounts of time and money rebuilding it from scratch. And that’s when the entropy moths move in, quietly chewing through whatever scraps remain: half-labeled folders, orphaned spreadsheets, and undocumented assumptions, until “we definitely learned something here” becomes “we have no idea what this means.”

That’s why a well-maintained, searchable knowledgebase isn’t a “nice to have”. It’s vital infrastructure. The catch is that continuous improvement is inherently ad hoc. Protocols vary, outputs are not standardized, and important metadata is often scattered across OneNote, meeting minutes, and oral tradition. Archiving this is non-trivial: the system needs to be searchable and auditable without stripping away the data richness that makes it worth keeping around.

This was a core problem that I was solving in my previous role at Bayer, and more recently with my own projects. I recently built a system to run systematic backtests across large parameter grids, which naturally needed a similar knowledgebase. In this post, I’ll show how to moth-proof your experiments against entropy by building an archive that captures minimum viable metadata, enforces a consistent storage “shape,” and keeps an auditable thread from figure to dataset to run to protocol.

## Why does knowledge go missing?
Most archives don’t fail catastrophically; they get eroded slowly. Usually there are a few modes of failure:

-	The test exists, but the context does not (why did we perform the test?)
-	The protocol exists, but metadata does not (what product did we test on? Who operated it? What version of automation did we do it with?)
-	Results exist, but the interpretation does not (were the results good? Why?)
-	Files existed on a person’s personal drive, but got deleted by a retention policy
-	Multiple floating versions of a slide deck or report (Atest_final_FINAL_v7.pptx)

Sometimes we have combinations of these scenarios, where the context for a follow up protocol is lost because of shared drive, or there are fundamental changes to methods that are lost. 

Now that the stakes are clear, let’s build an archive the entropy moths can’t eat!

## Defining the archive – minimum viable metadata
Unless we structure our thinking around protocols and testing, it is impossible to archive them. 

Every protocol starts with a research question. It usually takes the form of: “How can I modify X to get Y benefit from Z process?” Inherent to this question, we have a few of the KEY items we need to make our archive work. What we are testing? (modifying X), why are we doing it? (get y benefit), what is the context? (from Z process). While this is very simple, most testing follows this very pattern. We are already halfway there to defining the minimum viable metadata that we need to have a searchable archive. Here’s a quick reference of what this consists of.

**Table 1. Examples of  minimum viable metadata and why we need them** 

|     Category                 |     Examples  |     Purpose                                                                                                 |   
|:---|:----|:----|
|     Identity                 |     Protocol ID<br>     Title <br>    Owner<br>     Date                                                                                       |     Identifies   the protocol from similar ones to reduce confusion.                                        |
|     Linkages                 |     Related   testing<br>     Followups<br>     Tags                                                                                      |     Links tests   together so that finding context is easier without needing to provide redundant   data.    |
|     Justification            |     Hypothesis<br>     Business case<br>     Success   criteria                                                                            |     Gives context   as to why the test was performed.                                                       |
|     Inputs                   |     Materials<br>     Parameter   space                                                                                                |     Gives detail   on what was tested and important context for future design of experiments.                |
|     Methods                  |     Operating procedures<br>     Equipment&nbsp;and&nbsp;versions<br>     Sampling   methods<br>     Assays conducted                                  |     Gives context   on what may have changed and if the results are still valid.                          |
|     Knowledge   Generated    |     Conclusions<br>     Recommendations<br>     Edge cases<br>     Key   discussion<br>     Anticipated issues    |     This is what   we are trying to preserve.                                                                |
		

This data is best stored in a sql database, however you can make excel or sharepoint work depending on your organization’s preexisting technology stack. 

## The square peg / round hole fallacy – standardize the archiving, not the experiments 
Now that we have defined our non-negotiable data points which give our archive structure, you’re probably wondering how we are going to take all the rest of the data which usually has VERY LITTLE in common between tests and force them into the same database structure. The answer is – you don’t. 

You are going to standardize the envelope of the data rather than the content. What this means is you standardize how stuff is stored. Here’s an example file system based on the protocol identification. 
```
Archive/
  ├─ 20260101_testing_x_y_z/
  │  ├─ 20260101_protocol.docx
  │  ├─ 20260101_runs.csv
  │  ├─ 20260101_results.pptx
  │  ├─ 20260101_data.xlsx
  │  └─ rawdata/
  │     ├─ 20260101_assay1.xlsx
  │     ├─ 20260101_assay2.xlsx
  │     └─ 20260101_statistics.ipynb
  ├─ 20251201_testing_a_b_c/
  │  └─ …
  └─ …
```
This way, even if every test is different, the path to find the data is always the same.		

Congratulations, if you implemented the last 2 steps, you have a functional albeit basic archive. The challenge now is improving searchability, making it auditable, and getting people to use it. 

## If you want to find it, tag it – improving searchability
If you are familiar with SEO, you will know that good tagging is the cornerstone of making any content pop up in searches. The simplest system for tagging experiments is by using free tags. 

Free tags are liberating in that there is no rigidity, it is easy to implement and as your areas of study expand, you can expand the tag space without any additional development time and work on the archive system. However, in a system where you may have multiple groups or individuals with their own lexicon for the same phenomena this has the potential to spiral into being useless.

On the opposite end of the spectrum, if you use only controlled vocabulary (a dropdown) with rigid validation, you need more data management and development resources to grow the archive to fit the growing research needs. Once again, making the tagging system useless without the development resources. 

This calls for a hybrid solution, where you add a few tags that are required and validated fields in the metadata database. These are tags you know will likely be associated with all tests such as process, test type, material type – with the option of being left as Not Listed. The remainder of the tags should be in free tag form to allow expansion of the system as needed. 

Depending on the span of the knowledgebase, it may be worth reviewing experiments on a set time span (Annually or Semi-Annually) and making sure that tags align between departments and experimental groups.

## If you can’t trace it, you can’t trust it – design for auditability 
Unless you trust the data’s integrity, you cannot trust any conclusions you draw from it. The notion of auditability is making sure that the data is traceable and reproducible. This is where good data management principles come into play.

Raw data needs to be immutable or at least dated and signed. Most lab notebooks allow for signing and dating raw data and observations. In excel forms, it is best practice to include a date and signature at the bottom of the data file. 

All the data included in figures need to be traceable: figure ->  dataset ->  run ->  protocol. This allows reproduction of statistics and reuse of the data in comparison or meta studies. This is especially useful when tracking BOM (bill of materials) changes and changes in product quality – as you can iteratively look at deviations in quality metrics with each BOM change experiment. 

All deviations from protocol need to be recorded. Anything from as impactful as “We excluded runs x-y because data from previous runs shows no issues” to “we used the wrong sample ID” need to be recorded. These deviations may be overlooked in the future leading to wrong or misleading analysis in meta studies. 

Protocols need to be versioned. Like deviations from the protocol, changes to the protocol need to be recorded. Ensure that the protocol placed in the archive is the one that was actually followed. 

Assay methods and analysis scripts need to be versioned. Changes to analysis scripts, statistical methods, or assay methodology needs to be documented and versioned so that there is context on the results. Not having documentation here can lead to misleading comparisons in future studies. 

These rules are best enforced at the time of protocol archiving – that is right after the study is concluded and results are published. If you wait, details fade and the moths start nibbling at your context.

## Making the horse drink – driving adoption
You can bring the horse to the river but you cannot force it to drink. The most challenging part of designing any system is getting people to use it. If you followed along to this point, we are left with a very robust archive system that is very searchable and maintains good data management principles. The downside of being robust is that it is heavy on additional work. 

The best way to reduce friction and get your research teams to use it is to add in a good user interface. This is twofold, both the search functionality and the data entry methods need to be optimized.

Arguably, the more important of the two is the search function. All of the tooling you built for tagging and metadata are useless unless your search engine can parse through the data and retrieve relevant results. This can be as simple as the search/filter tooling on Sharepoint or a quick webapp, or as complicated as using an LLM trained on the data. 

And now the counterargument: there won’t be any data to be searched if you can’t get your teams to input it. You need to develop a good UI that allows you to have validated dropdowns and defaults keyed in to reduce time spent entering data.  This can be as simple as a quick PowerApp or Form which gives a few dropdowns and has a few defaults in there to speed up the entry process. Once again, depending on resources, you could outsource this process to an LLM, or at least the tagging. 


**WORD OF CAUTION:** LLMs enhance discoverability and ease of tagging; but does not substitute for good data management. 

## Keeping the entropy moths out...
A good archive does not try to turn every experiment into a standardized form. It does something far simpler and significantly more powerful: it makes every experiment findable, traceable, and reuseable. By capturing the minimum viable metadata, enforcing a consistent storage method, and layering in a hybrid tagging system, you build a knowledgebase that survives team changes, retention policies and the slow erosion of context. Auditability closes the loop by ensuring that results can always be walked backward: from a figure to the protocol that generated them so future comparisons don’t become mythology.

The last challenge is the most human one: adoption. The system needs to feel like a shortcut to research, not a chore. When the search works well, data entry is fast and when teams can reliably reuse past work your archive transforms from being just documentation and starts being a compounding asset. 
 
**Moth-proofed at last.**

<small> **genAI declaration:** The text matter of this blog post was completed without the use of generative AI tools. Cover image created using OpenAI’s image generation tool (DALL·E). Prompt: Library of scrolls with a jumble of papers on table that are being eaten by psychadelic colored moths. Time distortion effect in the background. Semi-messy composition, blog cover illustration, landscape (1920x1080). - Abhishek Benny.</small>