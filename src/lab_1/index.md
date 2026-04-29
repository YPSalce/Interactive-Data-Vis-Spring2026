---

## Lab 1: Passing Pollinators
toc: true

## Body Mass and Wing Span
<br>

```js
import * as Plot from "npm:@observablehq/plot";

const bees = FileAttachment("./data/pollinator_activity_data.csv").csv({ typed: true });
```

```js
function bodymassPlot(bees, {width} = {}) 
{
   return Plot.plot({
  title: "Body Mass Distribution of Pollinators",
  width: 700,
  height: 300,
  marginLeft: 60,
  x: { label: "Body Mass (g)" },
  y: { label: "Count", grid: true },
  color: { legend: true},
  marks: [
    Plot.rectY(
      bees,
      Plot.binX(
        { y: "count" },
        {
          x: "avg_body_mass_g",
          fill: "pollinator_species",
          thresholds: 10,
          inset: 0.5,
          tip: true
        }
      )
    ), 
    Plot.ruleY([0])
  ]
});
}

display (bodymassPlot(bees));
```

<br>

<div class ="card">
Honeybees tend to have smaller body masses compared to the other species, with their distribution concentrated at lower values. In contrast, carpenter bees exhibit larger body masses and a wider distribution, indicating greater variability in size. Bumblebees fall in between, with a moderate range of body masses.
</div>

<br>

```js

function wingspanPlot(bees, {width} = {}) 
{
   return Plot.plot({
  title: "Wing Span Distribution of Pollinators",
  width: 700,
  height: 300,
  marginLeft: 60,
  x: { label: "Wing Span (mm)" },
  y: { label: "Count", grid: true },
  color: { legend: true},
  marks: [
    Plot.rectY(
      bees,
      Plot.binX(
        { y: "count" },
        {
          x: "avg_wing_span_mm",
          fill: "pollinator_species",
          thresholds: 10,
          inset: 0.5,
          tip: true
        }
      )
    ), 
    Plot.ruleY([0])
  ]
});
}

display (wingspanPlot(bees));

```

<br>


<div class ="card">
Honeybees tend to have smaller wingspans, with their distribution concentrated at lower values, which is consistent with their smaller body mass observed above. Carpenter bees generally have the largest wingspans, while bumblebees fall in an intermediate range. However, there is notable overlap between carpenter bees and bumblebees, indicating that their wingspan sizes are not entirely distinct across species.
</div>

<br>

## Ideal Weather for Pollinating

<br>

```js
display(
  Plot.plot({
    title: "Pollination Activity by Weather Condition",
    width: 700,
    height: 300,
    marginLeft: 60,
    x: { label: "Weather Condition" },
    y: { label: "Average Pollination Activity", grid: true },
    marks: [
      Plot.barY(
        bees,
        Plot.groupX(
          { y: "mean" },
          {
            x: "weather_condition",
            y: "visit_count",
            fill: "weather_condition",
            tip: true
          }
        )
      ),
      Plot.ruleY([0])
    ]
  })
)
```
<br>

<div class ="card">
Average pollination activity is highest under cloudy conditions, followed by partly cloudy weather, and lowest under sunny conditions. This suggests that pollinators may be more active in less intense sunlight, possibly due to more favorable environmental conditions.
</div>

<br>

```js
display(
  Plot.plot({
    title: "Temperature Distribution During Pollination Activity",
    width: 700,
    height: 300,
    marginLeft: 60,
   
    x: { label: "Temperature in Celsius" },
    y: { label: "Count", grid: true },
    marks: [
      Plot.rectY(
        bees.filter(d => d.visit_count > 0),
        Plot.binX(
          { y: "count" },
          {
            x: "temperature",
            thresholds: 10,
            inset: 0.5,
            tip: true
          }
        )
      ),
      Plot.ruleY([0])
    ]
  })
)
```
<br>

<div class ="card">
Most visits happened between 20 and 28 degrees Celsius. 
</div>
<div class ="card">
Overall, pollination activity appears to be highest under cloudy conditions and within a moderate temperature range of approximately 20°C to 28°C. This suggests that pollinators are most active in stable, mild weather rather than in hot, sunny conditions.
</div>

<br>

## Nectar Production

<br>

```js
display(
  Plot.plot({
    title: "Nectar Production by Flower Species",
    width: 700,
    height: 400,
    marginLeft: 60,
    x: { label: "Flower Species" },
    y: { label: "Average Nectar Production", grid: true },
    color: { legend: true },
    marks: [
      Plot.barY(
        bees,
        Plot.groupX(
          { y: "sum" },
          {
            x: "flower_species",
            y: "nectar_production",
            fill: "flower_species",
            tip: true
          }
        )
      ),
      Plot.ruleY([0])
    ]
  })
)
```

<div class ="card">

The sunflower has the most nectar production with a total of 84.44 microliters (μL) of nectar. While the Coneflower had 69.33 μL and the lavender flowers had a total of 60.61 μL. 

</div>

