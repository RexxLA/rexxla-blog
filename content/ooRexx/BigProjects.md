---
title: "Rexx & Big Projects"
date: 2022-09-18T18:36:46+02:00
tags: []
featured_image: ""
description: ""
---
Listening to some of the Rexx Symposium last week was a bit of a throwback now that my career has moved on from software developer to novelist. Yeah, that’s a radical right turn, but probably a story for a different blog post.
The Q&A with Mike Cowlishaw always brings up some interesting topics, but this year, one question in particular struck a chord for me. The questioner asked something to the effect of whether or not Mike actually envisioned Rexx being used for large implementations. Of course, the answer was “yes,” but since we were nearing the end of the session, the conversation didn’t delve into much detail.

So herewith a proof point for the suitability of Rexx as a development language for applications of any size – including quite substantial ones. 

The company I worked for at the time was small and agile and had carved out a market niche for itself in software to enable skills transfer across multiple platforms. One of its earliest offerings was an implementation of Rexx for the numerous proprietary implementations of Unix that were being offered by the various hardware vendors. The product was a commercial success, but it also became the internal development language of choice for other offerings.

Perhaps the most significant of these – and certainly the most ambitious – was an implementation of ISPF.  You might be forgiven for thinking this would require complex C or assembly language code, and yes, there was C code for the underpinnings, including establishing the Rexx addressable environment. But enormous chunks of the rest of the product – panel definitions, execution of user input from panels, utilities, and much other functionality – was written entirely in Rexx.

The product was offered both in a basic version and an extended version that embedded Rexx and exposed that interface so that customers’ programming teams could further customize the environment quickly and easily. As with the Rexx offering itself, this ISPF look-alike was a commercial success. But our choice to use Rexx heavily for the implementation meant that any of our developers could quickly and easily diagnose customer issues and that any fix that might be required was easy to implement and was usually within a well-contained subset of code. That, in turn, made for efficient testing since a fix was well isolated and rarely had any potential to impact other areas of the product.

It was also easy to cobble together a prototype for customer-requested functionality and, once the prototype was validated, develop the full implementation in almost a self-documenting fashion – although, of course, we used comments extensively to simplify any future maintenance.

This particular project was undertaken at the time object-oriented programming was only just emerging from research centers like PARC and was not yet widely adopted in commercial software, so the Rexx we used was the classic language. Would we have made the same choices had C++ or Java been mainstream? I can only speak for myself, but I think the answer would have been “yes.”

Had we been targeting PC platforms, ooRexx would have undoubtedly been a good choice to deliver a proper GUI interface for end users. And once Java came along, I’m confident that, regardless of the target platform, we’d have embraced NetRexx as a way to achieve all the agility and ease-of-maintenance capabilities that we’d already demonstrated so definitively with our Classic Rexx implementation.

Pamela Taylor is a long-time member of the RexxLA Board of Directors. Though today, she writes more fiction than software code, she remains a strong advocate for Rexx in all its derivatives. You can find her historical fiction series, The Second Son Chronicles, in paperback, eBook, and audiobook on Amazon or wherever you buy books.