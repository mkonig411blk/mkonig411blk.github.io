---
layout: post
title:      "Approaching Complex Associations for Ruby Beginners"
date:       2019-08-15 23:34:18 -0400
permalink:  approaching_complex_associations_for_ruby_beginners
---


One of the concepts that I found most challenging in learning Rails for the first time was visualizing the web of models and how they are associated especially when the number of models and their corresponding associations increase. As someone who came into this with a familiarity (and level of comfort) around SQL and database relationships, I found that I could pretty quickly clear up any confusion by mapping out what the underlying tables may look like for the complex web. For example, I could easily identify whether a model, has_many, has_many through (requires a join table), or belongs_to another model when I  wrote out what the rows & columns in a table for each model would actually look like. 

Below is an example of the mapping out exercise I use prior to starting to code. Typically in addition to listing out the columns, I will  create rows with dummy data. For more details on the associations shown below, see my blog post about this specific Rails portfolio project: https://mkonig411blk.github.io/my_rails_portfolio_project_join_a_team

**Users** Table Columns (**has many** teams **through** signups):
* id 
* name
* email
* password_digest
* height
* sport city

**Teams** Table Columns (**has many** users **through** signups & **has many** matches):
* id
* name
* sport
* city

**Signups** Table Columns (**belongs to** a team & **belongs to** a user):
* id
* team_id
* user_id
* skill_level

**Matches** Table Columns (**belongs to** 2 teams - override to associate to team table):
* id
* location
* date
* home_team_id
* away_team_id

