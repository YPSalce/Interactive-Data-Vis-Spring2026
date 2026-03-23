---
title: "Lab 0: Getting Started"
toc: true
---

This page is where you can iterate. Follow the lab instructions in the [readme.md](./README.md).

# Interactive-Data-Vis-Spring2026

This is the home page of your new Observable Framework app.

For more, see [https://observablehq.com/framework/getting-started](https://observablehq.com/framework/getting-started).

# Getting Started 


<br> <h2> My Thoughts </h2>

Interactive Data Visualization has been a turning point for me.

I’ve been searching for a direction within data analysis that fuels both my curiosity and motivation—and this is it.

My objective is to build thoughtful interactive visualizations that empower people to explore information, discover insights, and ask meaningful questions. I also aim to use data as a storytelling tool to communicate ideas clearly and effectively.

This dashboard will serve as a record of my progress, capturing what I’ve learned and what I’ve created. 

<br> <h2> My Class and Lab Progress </h2>

This is the workflow I've been using to stay organized and consistent:  

<ul>
  <li>Complete readings before class</li>
  <li>Attend & participate in class</li> 
  <li>Watch lecture recording</li>
  <li>Annotate slides</li>
  <li>Finish assigned lab</li>
</ul>

Click the link below to see the class' Commons "Weekly Roundup" page. 

<a href="https://data73200spring2026.commons.gc.cuny.edu/weekly-roundup/">Visit Commons</a>

The checks in the completed column show that I completed everything for the week.

<table border="2">
  <tr> 
    <th> Class </th>
    <th> Date </th>
    <th> Segment </th>
    <th> Topic </th>
    <th> Completed </th>
  </tr>

  <tr> 
     <td> 1 </td>
     <td> 1/27 </td>
     <td> Theory </td>
     <td> What is interactive data visualization? </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>❌</span></td>
  </tr>

  <tr> 
     <td> 2 </td>
     <td> 2/3 </td>
     <td> Theory </td>
     <td> Visual Encodings, Perception, and Interactivity </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>✔️</span></td>
  </tr>
  <tr> 
     <td> 3 </td>
     <td> 2/10 </td>
     <td> Theory </td>
     <td> Applications of Interactive Data Visualizations </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>✔️</span></td>
  </tr>
  <tr> 
     <td> 4 </td>
     <td> 2/24 </td>
     <td> Code </td>
     <td> Intro to Web Development </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>✔️</span></td>
  </tr>
  <tr> 
     <td> 5 </td>
     <td> 3/3 </td>
     <td> Code </td>
     <td> Intro to Observable Framework </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>✔️</span></td>
  </tr>
  <tr> 
     <td> 6 </td>
     <td> 3/10 </td>
     <td> Code </td>
     <td> Plots, Scales and Marks </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>❌</span></td>
  </tr>
  <tr> 
     <td> 7 </td>
     <td> 3/17 </td>
     <td> Code </td>
     <td> Data Manipulation </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>❌</span></td>
  </tr>
  <tr> 
     <td> 8 </td>
     <td> 3/24 </td>
     <td> Code </td>
     <td> Data Types <> Scales <> Marks </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>❌</span></td>
  </tr>
  <tr> 
     <td> 9 </td>
     <td> 3/31 </td>
     <td> Code </td>
     <td> Annotations, Data Joins </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>❌</span></td>
  </tr>
  <tr> 
     <td> 10 </td>
     <td> 4/14 </td>
     <td> Code </td>
     <td> Facets, SVG, Maps </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>❌</span></td>
  </tr>
  <tr> 
     <td> 11 </td>
     <td> 4/28 </td>
     <td> Theory </td>
     <td> Evolution of Visualization Technologies </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>❌</span></td>
  </tr>
  <tr> 
     <td> 12 </td>
     <td> 5/5 </td>
     <td> Code </td>
     <td> Intro to D3 </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>❌</span></td>
  </tr>
  <tr> 
     <td> 13 </td>
     <td> 5/12 </td>
     <td> Code </td>
     <td> Advanced Visualizations </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>❌</span></td>
  </tr>
  <tr> 
     <td> 14 </td>
     <td> 5/19 </td>
     <td> Code </td>
     <td> Data Art </td>
     <td><button class="mark-done-btn">Mark Complete</button> <span>❌</span></td>
  </tr>
</table>

```js
for (const button of document.querySelectorAll(".mark-done-btn")) {
  button.addEventListener("click", () => {
    const status = button.nextElementSibling;
    status.textContent = status.textContent.trim() === "❌" ? "✔️" : "❌";
  });
}
```

  

