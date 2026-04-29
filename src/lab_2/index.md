---
title: "Lab 2: Subway Staffing"
toc: true
---

This page is where you can iterate. Follow the lab instructions in the [readme.md](./README.md).



```js
const incidents = FileAttachment("./data/incidents.csv").csv({ typed: true })
const local_events = FileAttachment("./data/local_events.csv").csv({ typed: true })
const upcoming_events = FileAttachment("./data/upcoming_events.csv").csv({ typed: true })
const ridership = FileAttachment("./data/ridership.csv").csv({ typed: true })
```


<div class = "card">

#### Original Data Sets


```js
const currentStaffing = {
  "Times Sq-42 St": 19,
  "Grand Central-42 St": 18,
  "34 St-Penn Station": 15,
  "14 St-Union Sq": 4,
  "Fulton St": 17,
  "42 St-Port Authority": 14,
  "Herald Sq-34 St": 15,
  "Canal St": 4,
  "59 St-Columbus Circle": 6,
  "125 St": 7,
  "96 St": 19,
  "86 St": 19,
  "72 St": 10,
  "66 St-Lincoln Center": 15,
  "50 St": 20,
  "28 St": 13,
  "23 St": 8,
  "Christopher St": 15,
  "Houston St": 18,
  "Spring St": 12,
  "Chambers St": 18,
  "Wall St": 9,
  "Bowling Green": 6,
  "West 4 St-Wash Sq": 4,
  "Astor Pl": 7
}
```

```js
display(currentStaffing)
display (incidents), 
display (local_events), 
display (upcoming_events), 
display (ridership)
```
</div>


#### How did local events impact ridership in summer 2025? 
#### What effect did the July 15th fare increase have?


```js 
const locevnts_w_rdrshp = ridership.map(rider => {
  const matchStation = local_events.find(event =>
    event.date.getTime() === rider.date.getTime() &&
    event.nearby_station === rider.station
  );

  return {
    ...rider,
    event_name: matchStation ? matchStation.event_name : null,
    attendance: matchStation ? matchStation.estimated_attendance : 0,
    has_event: matchStation ? true : false
  };
});
 ```

```js 
const locevnts_w_rdrshp2 = locevnts_w_rdrshp.map(rider2 => ({
  ...rider2,
  total_riders: rider2.entrances + rider2.exits,
  event_label: rider2.has_event ? "Event Day" : "No Event"
}));
```

<div class = "card">

#### Joined Data Set 1: Ridership and Local Events

```js
 display(locevnts_w_rdrshp)
```
#### Joined Data Set 2: Total Riders and Local Events

```js
display(locevnts_w_rdrshp2)
```
</div>

<div class = "card">

```js
Plot.plot({
title: "Average Ridership",
  width: 900,
  height: 400,
  marginLeft: 60,

  y: { label: "Average Ridership", grid: true},
  x: { label: "Event vs No Event" },
  color: {
    legend: true},

  marks: [
    Plot.barY(
      locevnts_w_rdrshp2,
      Plot.groupX(
        { y: "mean" },
        { x: "event_label", y: "total_riders", fill: "event_label"}
        )
      ),
      Plot.ruleY([0])
    ]
  })
```

As expected, average subway ridership was higher on event days.

</div>


<div class = "card">

**Aggregated Data Set: Total Daily Ridership Per Day**

```js
const daily_rdrship = Array.from(
  d3.rollup(
    locevnts_w_rdrshp2,
    rows => ({
      y: d3.sum(rows, d => d.total_riders),
      attendance: d3.sum(rows, d => d.attendance),
      has_event: rows.some(d => d.has_event),
      event_name: rows.find(d => d.has_event)?.event_name ?? null
    }),
    d => d.date
  ),
  ([date, value]) => ({ date, ...value })
).sort((a, b) => a.date - b.date)
```  

```js
display(daily_rdrship)
```

I aggregated the total ridership per date.

</div>


<div class = "card">

```js
Plot.plot({
  title: "Daily Subway Ridership in Summer 2025",
  width: 900,
  height: 400,
  marginLeft: 60,
  y: { label: "Total Daily Ridership", grid: true },
  x: { label: "Date" },
  color: { legend: true },
  marks: [
    Plot.lineY(
      daily_rdrship,
      {
        x: "date",
        y: "y",
        tip: true
      }
    ),
    Plot.dot(
      daily_rdrship, 
      {
        x: "date", 
        y: "y",
        r: 2.5,
        fill: "blue", 
        opacity: 0.7, 
        tip: true, 
        channels: {
          Date: "date",
          "Total Ridership": "y"
        }
      }
    ), 
    Plot.dot(
      daily_rdrship.filter(d => d.has_event),
      {
        x: "date",
        y: "y",
        fill: "purple",
        r: d => Math.max(3, Math.sqrt(d.attendance) / 20),
        tip: true,
        channels: {
          Event: "event_name",
          "Event Attendance": "attendance",
          "Total Ridership": "y"
        }
      }
    ),
    Plot.ruleX(
      [new Date("2025-07-15")],
      { stroke: "pink", strokeWidth: 3, strokeDasharray: "4,2" }
    ),
    Plot.textX(
      [{ date: new Date("2025-07-15"), label: "Fare Increase" }],
      { x: "date", text: "label", dy: 35 }
    )
  ]
})
```

Overall, on event days there seems to be higher ridership. However, not all event days saw higher subway ridership. This can indicate that there are other factors that drive ridership which may be worth exploring.

Additionally, the total number of riders on each day significantly decreased after the July 15 fare increase despite several more events scheduled for the rest of the summer. This can indicate that the increase in fare could have affected riders choice in taking the subway for events. We need more data to determine whether the fare increase caused this notable decrease in ridership during event days or whether the end of the summer has this general effect. 

</div>

#### How do the stations compare when it comes to response time? Which are the best, which are the worst?

<div class = "card">

```js
Plot.plot({
  title: "Incident Response Time by Station",
  width: 900,
  height: 400,
  marginLeft: 150,
  y: { 
    label: "Station", 
    grid: true, 
  },
  x: { label: "Average Response Time (minutes)" },
  marks: [ 
    Plot.barX(
      incidents,
      Plot.groupY( 
        { x: "mean" },
        { y: "station", 
          x: "response_time_minutes",
          fill: "station", 
          tip: true,
          sort: { y: "x", reverse: true }  
        }
      )
    ), 
    Plot.ruleX([0])
  ]
})
```
At the top of this graph, we see the stations with the worse reponse times. As we travel down the graph, we see stations with better response times. 

The worse stations have an average response time of approximately 18 to 19 minutes. Some of those stations are: 

1. 59 St. Colombus Circle (19.0 mins)
2. West 4th St. Washington Sq. (18.71 mins)
3. 125th St. (18.5 mins)
4. Bowling Green (18.5 mins)
5. Canal Street (18.4 mins)

The best stations have an average response time of approximately 5 minutes. Some of those stations are 

1. Fulton St. (5.014 mins) 
2. Houston St. (5.036 mins) 
3. Time Square - 42nd St. (5.119 mins)
4. 34th St.- Penn Station (5.148 mins)
5. 86th St. (5.231 mins) 

</div>

#### Which three stations need the most staffing help for next summer based on the 2026 event calendar?
<div class = "card">

**Aggregated Upcoming Events by Station: Events Total Expected Attendance and Count for Each Station**

```js 
const upcoming_by_station = Array.from(
  d3.group(upcoming_events, d => d.nearby_station),
  ([station, rows]) => ({
    station: station,
    total_expected_attendance: rows.reduce((sum, d) => sum + d.expected_attendance, 0),
    event_count: rows.length
  })
);
```

```js
display (upcoming_by_station)
```

**Joined Data Set 3:**

```js
const upcomingStation_w_staffing = upcoming_by_station.map(event => {
  const staffing = currentStaffing[event.station];

  return {
    ...event,
    staffing_count: staffing ? staffing : 0
  };
});
```

```js
display (upcomingStation_w_staffing)
```

I created a metric to measure station staffing need. 

Since it makes sense to assume that higher station ridership requires more staffing, I defined each station's need as the total expected attendance in 2026 divided by the amount of current staff.  

**Data Set with Staffing Need:**

```js
const staffing_need = upcomingStation_w_staffing.map(d => ({
  ...d,
  need_measure: d.total_expected_attendance / d.staffing_count
}));
```

```js 
display (staffing_need)
```
</div>

<div class = "card">

```js
Plot.plot({
  title: "Stations Most in Need of Staffing for Summer 2026",
  width: 900, 
  height: 500, 
  marginLeft: 150, 
  y: {label: "Station"},
  x: {label: "Attendance per Current Staff Member"},
  marks: [
    Plot.barX(
    staffing_need, 
    {
      x: "need_measure", 
      y: "station", 
      fill: "station", 
      tip: true,
      sort: {y: "x", reverse: true}
    }  
   )
  ]
})
```

According to this graph, the station with the highest need is Canal St because it expects a total of 70,193 riders across 8 events scheduled near it, but only counts with 4 staff members. The next stations showing the most need are: 

1. 34th St. - Penn Station (Total Expected Attendance = 59,378 potential riders, 8 events, 15 staff members, need score = 3958 riders per staff member  )
2. 23 St. (Total Expected Attendance = 24,897 potential riders, 3 events, 8 staff members, need score = 3,112 riders per staff member)
</div>



#### If you had to prioritize _one_ station to get increased staffing, which would it be and why?

<div class = "card" 

I would prioritize Canal Street to get more staff because it has the highest need score. Canal St. expects the highest traffic yet has the least amount of staff members in the station. There are other stations that only have 4 staff members, but they don't expect the same level of traffic in 2026. 

</div>
