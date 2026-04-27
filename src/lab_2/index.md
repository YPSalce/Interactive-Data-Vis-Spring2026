---

## title: "Lab 2: Subway Staffing"

toc: true

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

```js
Plot.plot({
title: "Average Ridership",
  width: 900,
  height: 400,
  marginLeft: 60,

  y: { label: "Average Ridership", grid: true},
  x: { label: "Date" },
  color: {
    legend: true},

  marks: [
    Plot.lineY(
      locevnts_w_rdrshp2,
      Plot.groupX(
        { y: "mean" },
        { x: "date", y: "total_riders", stroke: "event_label"}  
      )),
      Plot.ruleX(
        [new Date("2025-07-15")]),
      Plot.textX(
      [{ date: new Date("2025-07-15"), label: "Fare Increase" }],
      { x: "date", text: "label", dy: -10 }
    )
    ]
  })
```

As expected, average subway ridership was higher on event days.

</div>