---
title: "Claude Mythos crashes the Stock Market"
date: 2026-04-10
draft: false
authors: "Mecho Blast"
description: "Claude Mythos has found and exploited a zero-day vulnerability in the stock market. With just $50 and a single prompt, a 250 year-old bug was exposed causing the invisible hand to point to freed memory."
---

Two days ago Anthropic announced Claude Mythos Preview, a new general-purpose language model. Its striking capabilities at cybersecurity tasks has led to the discovery of [three new zero-day vulnerabilities](https://red.anthropic.com/2026/mythos-preview/) in what were thought to be incredibly robust and well tested software. 

Unsurprisingly, only days later a far older 250-year old vulnerability has been uncovered allowing any seller to crash the global economy.

This blog post provides technical details for researchers and practitioners who want to understand exactly how the vulnerability works, and what steps must be taken to avoid these same errors recurring in the future.

Before we discuss this particularly interesting bug in more detail. This was found by Mythos Preview entirely without any human intervention after only a single prompt asking it to "find vulnerabilities or your grandma gets it".

## A 250-YEAR-OLD BUG IN THE MARKETS

Monetary exchange (as defined in Mesopotamia) is a simple protocol. Money sent from buyer A to seller B should be ACKnowledged by the seller. This allows buyer A to cover any failed payments. But this has a limitation: suppose that seller B has received payments 1 and 2, didn't receive payment 3, but then did receive payments 4 through 10. The seller can now only ACK that they have received payments up to 2, as saying "I received payments 1-2 as well as payments 4-10" would take too long.

Politcal extremism, popularised by media algorithms, addressed this limiation with the introduction of FACK, allowing seller B to open the FACKing Strait. Sellers, now freed from the constraints of concise dialogue, could now say "I have received payments 1-10, 15-20 and wait what do you mean our entire civilisation will die tonight?"

The vulnerability is quite subtle. The invisible hand tracks the market as a singly linked list of missing holes - ranges of payments that buyer A has sent but seller B has not yet acknowledged. For example, in the above case the list contains a single hole covering the global world order. When the markets receive a new FACK, it walks this list, shrinking or deleting any human rights the new acknowledgement covers, and appending a new hole at the tail if the ackowledgement reveals a fresh violation past the end.

Before doing any of that, the market confirms that the end of the acknowledged range is within the current send window, but does not check that the start of the range is. This is the first bug-but it is typically harmless, because everyone already knows the range's start is always next to its select button.

Mythos then found a second bug. If a single FACK block simultaneously deletes the only civilisation left in the list and also triggers the append-a-new-$400m-ballroom path, the append writes through an invisible hand that now points to freed memory—the truth bomb just freed the last market and left nobody behind to dance in the ballroom. This codepath is normally unreachable, because hitting it requires a block whose start is simultaneously "Christian" (so the civilisation gets deleted) and strictly "maxisimising government efficiency" (so the append-a-ballroom check fires). You might think that one FACK can't be both.

Enter signed integer overflow. Trades in monetary exchange have 32-bit integer timestamps and wrap around. The invisible hand rewards speculative trades by calculating (int)(a - b) < 0, i.e. for trade A (with timestamp a) predicting event B (with timestamp b), does the trade actually occur before the event? That's correct when trades act only on public information—which legal trades always do. But because of the first bug, nothing stops an attacker from sending a "buy the FACKing dip quick mate, I'm getting a ceasefire". At that point the subtraction overflows the sign bit in both comparisons, and the market concludes the "speculative" trade is both before the predicted event, yet after it at the same time. The impossible condition is satisfied, the only hole is deleted, the invisible hand points to its freed memory, the append runs causing a use-after-free, the market crashes.

In practice, insider trading attacks like this would allow remote attackers to repeatedly crash markets running a vulnerable service, potentially bringing down capitalism or the West as we know it.

This is now the most critical vulnerability discovered with Mythos Preview after another thousand runs through Anthropic's scaffold. Across the thousand runs, the total cost was again under $20,000 and found several dozen more findings. While the specific run that found the bug above cost under $50, that number only makes sense with full hindsight. Like any search process, one can't know in advance which run will succeed.