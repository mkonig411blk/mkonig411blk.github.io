---
layout: post
title:      "My Sinatra Portfolio Project: Trip Organizer"
date:       2019-07-14 00:56:53 -0400
permalink:  my_sinatra_portfolio_project_trip_organizer
---


With the lessons learned from my first portfolio project tucked in my back pocket, I was ready to embark on my second portfolio project using Sinatra, ActiveRecord, and my new knowledge of the popular application architecture, MVC (Model-View-Controller). I decided to create a Trip Organizer with users, trips, and activities as my models. 

My biggest takeaway this time around was to map out the user journey or workflow prior to diving into the code. The workflow was simple for my first project, a command line interface. This was the first time I tackled something end to end that required a little more pre-planning in terms of the user's journey. Once I started to play around in the application, I found that buttons appeared in certain places where it would not have made sense from a practical standpoint. For example, the Trip Organizer does not intend for the user to create activities without having any trips already created. All activities must be associated to a trip (or from a database perspective, to a trip_id). This behavior was controlled entirely in my views ("V" of the MVC), using the Embedded RuBy (ERB) templating engine, which comes standard with every Ruby installation. ERB and other templating engines allow us to modify the content and structure of our HTML code. 

My trips/index view (see below) displayed all existing trips for the user. I created an *if* statement using ERB scripting tags to determine whether the user's trips should show (@trips.length > 0) or whether to show a message indicating that no trips have been created yet. Outside of this *if* statement, I created the "Add Trip", "Add Activity", and "Logout" buttons. It soon became apparent, that to create the best user experience, the "Add Activity" button belonged inside that *if* statement. This ensured that the user could not create an activity without having at least 1 trip created.

```
<head>
    <title>Trip Organizer by MK</title>
</head>
<body>
    <h3>My Existing Trips:</h3>
    <ul>
        <% if @trips.length > 0
        @trips.each do |trip|%>
        <div>
            <a href="/trips/<%=trip.id%>">Trip Name:<strong> <%=trip.name%> in <%=trip.location%></strong></a>
            <li>From <%=trip.start_date%> to <%=trip.end_date%></li>
            <li>Attendee List: <%=trip.attendees%></li>
            <li>Transporation Method: <%=trip.transportation%></li>
        </div>
        <a href="/trips/<%= trip.id %>/edit">EDIT</a>
        <form action="/trips/<%= trip.id %>" method="post">
            <input id="hidden" type="hidden" name="_method" value="delete">
            <input type="submit" value="DELETE">
        </form>
        <br>
        <%end%>
        <a href="/activities/new" class="button">CREATE ACTIVITY</a>
        <%else%>
        <h5>You have not created any trips yet!</h5>
        <%end%>
        <a href="/trips/new" class="button">CREATE TRIP</a>
        <br></br>
        <a href="/logout">LOGOUT</a>
    </ul>
</body>

```

This is just one example of a last minute change to my tool that with a little pre-planning and mapping out could have been incorporated into the original plan for my project. 

