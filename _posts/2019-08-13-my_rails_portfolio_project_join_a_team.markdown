---
layout: post
title:      "My Rails Portfolio Project: Join a Team"
date:       2019-08-13 01:26:13 -0400
permalink:  my_rails_portfolio_project_join_a_team
---


Our final project in Ruby challenged us to use the Rails framework to build out our most complex web application yet with requirements such as multiple different model associations & relationships, validations & 3rd party authentication (OmniAuth), ActiveRecord scope methods, nested routes, and more. It can be challenging alone to come up with an idea that fulfills those requirements while staying within a resonable knowledge scope for where we are in the course. 

I ran with the "Join a Team" idea, which allows **users** (model 1) to **signup** (model 2) for sports **teams** (model 3) in their area. Once on a team, users can challenge other teams to matches or "**games**" (model 4). The relationship between users, signups, and teams was straightforward and similar to relationships we freqently saw in labs with a domain model involving doctors, appointments, and patients. The signups table was the join table for teams & users, in other words, signups are what connect teams to players and vice versa. 

```
class Signup < ApplicationRecord
  belongs_to :team
  belongs_to :user
end

###

class Team < ApplicationRecord
    has_many :signups
    has_many :users, through: :signups
end

###

class User < ApplicationRecord
    has_many :signups
    has_many :teams, through: :signups
end
```

The challenge, however, came when trying to incorporate games into the picture. The unique situation for games, which I had not encountered before, was that games "have many" teams, but more specifically games always and only ever have 2 teams. Teams "have many" games. Instead of creating a join table for these two models, it actually made more sense to have 2 columns in the games table corresponding to: home_team_id and away_team_id. From there, the question was how to use ActiveRecord properly to ensure that we could connect a home_team_id and an away_team_id to the teams table despite the nonconventional column names. 

The solution was to include foreign_keys pointing to the teams table in the migration as seen below. 

```
class CreateGames < ActiveRecord::Migration[5.2]
  def change
    create_table :games do |t|
      t.string :name
      t.string :city
      t.string :location
      t.datetime :date
      t.integer :home_team_id, foreign_key: { to_table: :teams}
      t.integer :away_team_id, foreign_key: { to_table: :teams}
    end
  end
end
```

As far as the existing models went, I added the simple "has_many :games" line to the team model and added the "belongs_to" assocations below to the game model.

```
class Game < ApplicationRecord
    belongs_to :home_team, class_name: "Team"
    belongs_to :away_team, class_name: "Team"
end
```

With the associations properly setup with ActiveRecord, I was able to simple pull the home & away team names in my views as needed. 

```
<%=@game.home_team.name %> (Home) vs. <%= @game.away_team.name %></span>
```
