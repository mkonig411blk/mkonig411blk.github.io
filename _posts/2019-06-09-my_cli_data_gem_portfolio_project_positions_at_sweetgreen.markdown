---
layout: post
title:      "My CLI Data Gem Portfolio Project: Positions at Sweetgreen"
date:       2019-06-09 18:41:42 -0400
permalink:  my_cli_data_gem_portfolio_project_positions_at_sweetgreen
---


It was definitely exciting to complete my first portfolio project and was a great opportunity to assess any of my knowledge gaps as well as challenge myself to learn new concepts that resulted in a more powerful CLI. 

One of the approaches that I wanted to take that differed from the Music Library CLI Lab was to create instances of all 3 of my classes within one of the scraper methods. I also wanted to rely solely on the @@all arrays for the 3 classes when using a class instance to lookup all instances in another class with that attribute (e.g. show me all the positions in the Houston office at Sweetgreen). Instead the Music Library CLI lab created @songs arrays in both the Genre & Artist classes. I wanted to validate that I was able to get the same result with a different approach.

One of the most challenging aspects of creating 3 instances of 3 different classes in a single method was getting the ordering right. You can't assign an instance to another instance if the original instance doesn't exist! Ultimately, the ordering went as follows:
1. Assign the text for the position name to a variable
2. Assign the text for the department name to a variable
3. Assign the text for the office name to a variable
4. Initialize the position instance with its name variable (#1), these are inherently unique
5. Initialize the department instance with its name variable (#2) if it does not already exist
6. Initialize the office instance with its name variable (#3) if it does not already exist
7. Assign the department instance (#5) to the position instance (#4)
8. Assign the office instance (#6) to the position instance (#4)

```
  def make_classes
    self.get_rows.drop(1).each do |row|
      position_name = row.css("td.job_title").text
      department_name = row.css("td.job_department").text
      office_name = row.css("td.job_location").text
      position = Position.new(position_name)
      department = Department.find_or_create_by_name(department_name)
      office = Office.find_or_create_by_name(office_name)
      position.department = department
      position.office = office
    end
  end
```

As far relying on the @@all arrays for searching across classes, I was able to achieve this in a method in the CLI file. Below is a snippet within that method where I utilized the @@all array from the Position class (Position.all) to lookup any positions for the office inputed by the user. I created this same method for the Department class as well for users looking to search for positions in a particular department.

```
if office = Office.all.find {|off| off.name.downcase == input}
          Position.all.each do |pos| 
              if pos.office.name.downcase == input
                  puts "#{counter}. #{pos.name} - #{pos.department.name} in #{pos.office.name}" 
                  counter += 1
              else
              end
```
