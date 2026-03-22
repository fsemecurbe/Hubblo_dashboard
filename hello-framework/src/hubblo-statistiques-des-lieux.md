# Hubblo : Statistiques des lieux

Cliquez sur la droite pour définir la localisation de votre Hubblo ou localisez vous avec le bouton **Me localiser**.

```js
viewof rayon = Inputs.range([200, 1000], {label: "Rayon", step: 100})
```

```js
viewof container = html`<div style="position: relative;">
    <div id="map" style="height: 400px;"></div>
    <button style="
      position:absolute;
      top:10px;
      right:10px;
      z-index:1000;
      padding:6px 10px;
      background:white;
      border:1px solid #ccc;
      cursor:pointer;
    ">📍 Me localiser</button>
  </div>`
```

Le nombre de ménages du ${data['unit'][1]} est de ${Math.round(data['men'][1])}, celui de ${data['unit'][0]} est de ${Math.round(data['men'][0])}. La taille des ménages du ${data['unit'][1]} est de ${Math.round(100 * data['ind'][1] / data['men'][1])/100}, contre ${Math.round(100 * data['ind'][0] / data['men'][0])/100} pour ${data['unit'][0]}. Le revenu moyen du ${data['unit'][1]} est de ${Math.round(data['ind_snv'][1]/data['ind'][1])} contre ${Math.round(data['ind_snv'][0]/data['ind'][0])} pour ${data['unit'][0]}

```js
var data_famille = {
  
  const vars = ['men_pauv', 'men_1ind', 'men_5ind', 'men_prop', 'men_fmp', 'men_coll', 'men_mais'];
  const libelle = ['Pauvres', 'Une personne', 'Nombreuses', 'Propriétaires', 'Mono-parentales', 'En immeuble', 'En maison']
  
  const values0 = vars.map((element) => 100 * data[element][0] / data['men'][0]);
  const values1 = vars.map((element) => 100 * data[element][1] / data['men'][1]);

  var data_famille = []
  
 for (const vari in vars) {
  
 var dico = {}
 dico['indicateur'] = libelle[vari] 
 dico[data['unit'][0]] = values0[vari]  
 dico[data['unit'][1]] = values1[vari]   
 data_famille.push(dico) 
 }
return data_famille 
} 
```

```js
Plot.plot({
  title: "Familles", 
  width,
  height: 400,
  
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
```

```js

var data_logement = {
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
return data_logement 



}
```

```js

Plot.plot({
  title: "Logements", 
  width,
  height: 400,
  
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
```

```js
data_age = {
  
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
return data_age
  
}

```

```js

Plot.plot({
  title: "Âge des personnes", 
  width,
  height: 400,
  
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
```

```js
objectToTable(data)
```

## Annexes

```js
data = getFilosofi(coordinates3035[0], coordinates3035[1], rayon)
```

```js
function objectToTable(obj) {
  const table = document.createElement("table");
  table.border = "1";

  for (const [key, value] of Object.entries(obj)) {
    const row = document.createElement("tr");

    const nameCell = document.createElement("td");
    nameCell.textContent = key;

    const valueCell1 = document.createElement("td");
    valueCell1.textContent = value[0];

    const valueCell2 = document.createElement("td");
    valueCell2.textContent = value[1];
    
    row.appendChild(nameCell);
    row.appendChild(valueCell1);
    row.appendChild(valueCell2);



    
    table.appendChild(row);
  }

  return table;
}
```

```js
coordinates3035 = proj4('+proj=longlat +datum=WGS84 +no_defs +type=crs',
                        '+proj=laea +lat_0=52 +lon_0=10 +x_0=4321000 +y_0=3210000 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs +type=crs',
                        [toto.lng, toto.lat]);
```

```js
async function getFilosofi(x, y, radius) {
  const url = `https://hubblo.up.railway.app/filosofi?x=${x}&y=${y}&radius=${radius}`;

  const response = await fetch(url);
  const data = await response.json();

  return data['message'];
}


```

```js
proj4 = require("proj4")
```

```js
L = {
  const L = await require("leaflet@1.8/dist/leaflet.js");
  if (!L._style) {
    const href = await require.resolve("leaflet@1.8/dist/leaflet.css");
    document.head.appendChild(L._style = html`<link href=${href} rel=stylesheet>`);
  }
  return L;
}
```

```js echo
plt = require("https://cdn.plot.ly/plotly-latest.min.js")
```

```js echo
circle.setRadius(rayon)
```

```js
circle.addTo(map)
```

```js echo
viewof toto = LeafletMap2() 
```

```js echo
var map = {
  const map = L.map(container).setView([43.596, 1.4419], 13); // Paris

  L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", {
    attribution: "&copy; OpenStreetMap contributors"
  }).addTo(map);

  return map;
}
```

```js echo
var circle = L.circle([43.596, 1.4419], {radius : 200} )
```

```js echo
const LeafletMap2 = (value = { lat: 43.596, lng: 1.4419, rayon: 200 }) => {
  const { lat, lng, rayon } = value;
  const curValue = { lat, lng };
  const store = { value: curValue };



  

  

  function updatePosition(latlng) {
    store.value = latlng;
    circle.setLatLng(latlng);
    map.setView(latlng, 13);

    container.dispatchEvent(new Event("input", { bubbles: true }));
  }

  // 👉 clic sur la carte
  function onMapClick(e) {
    updatePosition(e.latlng);
  }
  map.on("click", onMapClick);

  
  Object.defineProperty(container, "value", {
    get() {
      return store.value;
    },
    set(v) {
      store.value = v;
    }
  });

  container.value = curValue;

  //return container;
};
```
