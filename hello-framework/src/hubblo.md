---
theme: dashboard  
toc: false
---


<div class="hero">
  <h1>Hubblo</h1>
</div>


```js
const rayon = view(
  Inputs.range(
    [100, 1000],
    {step: 100, label: "Rayon"}
  )
);
```



<div class="grid grid-cols-2" style="grid-auto-rows: 240px;">
  <div class="card" style="padding: 0;">
  <div id="map" style = "height: 100%; width: ${width}"></div>
  </div>
  <div class="card" style="padding: 0;">
    ${resize((width, height) => graph_famille(width, height))}
  </div>
</div>


<div class="grid grid-cols-2" style="grid-auto-rows: 240px;">
  <div class="card" style="padding: 0;">
    ${resize((width, height) => graph_logement(width, height))}
  </div>
  <div class="card" style="padding: 0;">
    ${resize((width, height) => graph_age(width, height))}
  </div>
</div>











```js
async function getFilosofi(x, y, radius) {
  const url = `https://hubblo.up.railway.app/filosofi?x=${x}&y=${y}&radius=${radius}`;

  const response = await fetch(url);
  const data = await response.json();

  return data['message'];
}
```


```js
var data = getFilosofi(3756295, 2889313, rayon)
```



```js
//div.map = "height: 400px;";

const map = L.map("map")
  .setView([43.596, 1.4419], 13);

L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
  attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a>'
})
  .addTo(map);

```



```js 
var circle = L.circle([43.596, 1.4419], {radius : 200} )
```


```js
circle.addTo(map)
```

```js 
circle.setRadius(rayon)
```



```js
var vars = ['men_pauv', 'men_1ind', 'men_5ind', 'men_prop', 'men_fmp', 'men_coll', 'men_mais'];
var libelle = ['Pauvres', 'Une personne', 'Nombreuses', 'Propriétaires', 'Mono-parentales', 'En immeuble', 'En maison']
  
var values0 = vars.map((element) => 100 * data[element][0] / data['men'][0]);
var values1 = vars.map((element) => 100 * data[element][1] / data['men'][1]);

var data_famille = []
  
for (const vari in vars) {
var dico = {}
dico['indicateur'] = libelle[vari] 
dico[data['unit'][0]] = values0[vari]  
dico[data['unit'][1]] = values1[vari]   
data_famille.push(dico) 
} 
```

```js
function graph_famille(width, height){ 
 return Plot.plot({
  title: "Familles", 
  label: "",
  width,
  height: height *.9,
  
  x: {
    name: ""
  },

  y: {
    axis: "left",
    grid: true,
    ticks: d3.range(0, Math.max(...data_famille.map(item => Math.max(item[data['unit'][0]], item[data['unit'][1]]))) , 10),
    tickFormat: (d) => (d === 50 ? `${d}%` : `${d}`),
    tickSize: 0
  },
  
  color: { scheme: "dark2" },
  
  
  
  marks: [
    
 
    Plot.barY(data_famille, {
      x: "indicateur",
      y: data['unit'][0],
      tip: true,
      fill: "#ddd",

      
      insetRight: 0,
      insetLeft: 0
    }),
    Plot.barY(data_famille, {
      x: "indicateur",
      y: data['unit'][1],
      tip: true,
      
      fill: "#add8e6",

      
      insetRight: 10,
      insetLeft: 10
      
    }),
    
    
    
    Plot.ruleY([0])
  ]
})
}
```

```js
var vars = ['log_av45', 'log_45_70', 'log_70_90', 'log_ap90'];
 const libelle = ['Avant 1945', '1945 et 1970', '1970 et 1990', 'Après 90' , 'Sociaux']
  const somme0 = vars
    .concat(['log_inc'])
    .map(k => data?.[k]?.[0] || 0)
    .reduce((a, b) => a + b, 0);

  const values0 = vars.map(k => {
    const v = data?.[k]?.[0] || 0;
    return somme0 ? 100*v / somme0 : 0;
  });

  values0.push(100*data['log_soc'][0] / somme0)
  
  const somme1 = vars
    .concat(['log_inc'])
    .map(k => data?.[k]?.[1] || 0)
    .reduce((a, b) => a + b, 0);

  const values1 = vars.map(k => {
    const v = data?.[k]?.[1] || 0;
    return somme1 ? 100*v / somme1 : 0;
  });

  values1.push(100*data['log_soc'][1] / somme1)

  var data_logement= []

 const vars2 = vars.concat(['log_soc'])
 for (const vari in vars2) {
  
 var dico = {}
 dico['indicateur'] = libelle[vari] 
 dico[data['unit'][0]] = values0[vari]  
 dico[data['unit'][1]] = values1[vari]   
 data_logement.push(dico) 
 }
```



```js
function graph_logement(width, height){ 
 return Plot.plot({
  //title: "Logements", 
  width,
  label: "",
  height: height ,
  
  y: {
    axis: "left",
    grid: true,
    ticks: d3.range(0, Math.max(...data_logement.map(item => Math.max(item[data['unit'][0]], item[data['unit'][1]]))) , 10),
    tickFormat: (d) => (d === 50 ? `${d}%` : `${d}`),
    tickSize: 0
  },
  
  color: { scheme: "dark2" },
  
  
  
  marks: [
    
 
    Plot.barY(data_logement, {
      x: "indicateur",
      y: data['unit'][0],
      tip: true,
      fill: "#ddd",

      
      insetRight: 0,
      insetLeft: 0
    }),
    Plot.barY(data_logement, {
      x: "indicateur",
      y: data['unit'][1],
      tip: true,      
      fill: "#add8e6",
      insetRight: 10,
      insetLeft: 10
      
    }),
    
    
    
    Plot.ruleY([0])
  ]
})
}
```

```js
var vars = ['ind_0_3', 'ind_4_5', 'ind_6_10', 'ind_18_24', 'ind_25_39',
              'ind_40_54', 'ind_55_64', 'ind_65_79', 'ind_80p']
var libelle =['0 - 3', '4 - 5', '6 - 10', '18 - 24', '25 - 39',
              '40 - 54', '55 - 64', '65 - 79', '> 80p']
  const somme0 = vars
    .concat(['ind_inc'])
    .map(k => data?.[k]?.[0] || 0)
    .reduce((a, b) => a + b, 0);

  const values0 = vars.map(k => {
    const v = data?.[k]?.[0] || 0;
    return somme0 ? v / somme0 : 0;
  });

  const somme1 = vars
    .concat(['ind_inc'])
    .map(k => data?.[k]?.[1] || 0)
    .reduce((a, b) => a + b, 0);

  const values1 = vars.map(k => {
    const v = data?.[k]?.[1] || 0;
    return somme1 ? v / somme1 : 0;
  });

  
  var data_age= []

 
 for (const vari in vars) {
  
 var dico = {}
 dico['indicateur'] = libelle[vari] 
 dico[data['unit'][0]] = values0[vari]  
 dico[data['unit'][1]] = values1[vari]   
 data_age.push(dico) 
 }


```



```js
function graph_age(width, height){ 
 return Plot.plot({
  //title: "Âge des personnes", 
  label: "",
  width,
  height: height,
  y: {
    axis: "left",
    grid: true,
    ticks: d3.range(0, Math.max(...data_age.map(item => Math.max(item[data['unit'][0]], item[data['unit'][1]]))) , 10),
    tickFormat: (d) => (d === 50 ? `${d}%` : `${d}`),
    tickSize: 0
  },
  color: { scheme: "dark2" }, 
  marks: [
    Plot.barY(data_age, {
      x: "indicateur",
      y: data['unit'][0],
      tip: true,
      fill: "#ddd",
      insetRight: 0,
      insetLeft: 0
    }),
    Plot.barY(data_age, {
      x: "indicateur",
      y: data['unit'][1],
      tip: true,
      fill: "#add8e6",
      insetRight: 10,
      insetLeft: 10      
    }),
    Plot.ruleY([0])
  ]
})
}
```


<style>

.hero h1 {
  margin: 1rem 0;
  padding: 1rem 0;
  max-width: none;
  font-size: 5vw;
  font-weight: 900;
  line-height: 1;
  background: linear-gradient(30deg, var(--theme-foreground-focus), currentColor);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}


</style>
