---
layout: post
title:      "Approaching Complex Associations for Ruby Beginners"
date:       2019-08-16 03:34:17 +0000
permalink:  approaching_complex_associations_for_ruby_beginners
---


One of the concepts that I found most challenging in learning Rails for the first time was visualizing the web of models and how they are associated especially when the number of models and their corresponding associations increase. As someone who came into this with a familiarity (and level of comfort) around SQL and database relationships, I found that I could pretty quickly clear up any confusion by mapping out what the underlying tables may look like for the complex web. For example, I could easily identify whether a model, has_many, has_many through (requires a join table), or belongs_to another model when I  wrote out what the rows & columns in a table for each model would actually look like. 
