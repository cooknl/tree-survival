---
sql:
  treeStatus: ./data/tree_status.csv
  fires: ./data/FTM_fires.csv
---

# Tree survival post-wildfire

## Data: Tree and wildfire data from the Fire and Tree Mortality Database (Cansler et al. 2020)

<https://www.nature.com/articles/s41597-020-0522-7>

```js
const treeSpecies = view(
  Inputs.radio(
    [
      "White fir",
      "Lodgepole pine",
      "Douglas fir",
    ],
    {value: "White fir", label: "Pick species:"}
  )
);
```

```sql id=treeFireJoin display
SELECT yr_fire_name as "Year - Fire Name"
  , common_name as "Common Name"
  , CAST(yr_post_fire AS INTEGER) as "Years Post Fire"
  , status
FROM treeStatus
JOIN fires ON fires.YrFireName = treeStatus.yr_fire_name
WHERE common_name = ${treeSpecies}
```

```js
const treeSurvival = Plot.plot({
  color: {legend: true},
  marks: [
    Plot.barY(treeFireJoin, Plot.groupX({y: "count"},
    {x: "Years Post Fire", fill: "status"}))
  ]
});

display(treeSurvival);
```
