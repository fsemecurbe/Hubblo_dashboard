# Hubblo

Connaitre la population autour de vous


<div class="grid grid-cols-2" style="grid-auto-rows: 240px;">
  <div class="card" style="padding: 0;">
    ${resize((width, height) => Plot.barY([9, 4, 8, 1, 11, 3, 4, 2, 7, 5]).plot({width, height}))}
  </div>
  <div class="card" style="padding: 0;">
    ${resize((width, height) => Plot.barY([3, 4, 2, 7, 5, 9, 4, 8, 1, 11]).plot({width, height}))}
  </div>
</div>















```js
const rayon = view(
  Inputs.range(
    [100, 1000],
    {step: 100, label: "Rayon"}
  )
);
```

```js
const div = display(document.createElement("div"));
div.style = "height: 400px;";

const map = L.map(div)
  .setView([43.596, 1.4419], 13);

L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
  attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a>'
})
  .addTo(map);

L.marker([43.596, 1.4419])
  .addTo(map)
  .bindPopup("A nice popup<br> indicating a point of interest.")
  .openPopup();
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
async function getFilosofi(x, y, radius) {
  const url = `https://hubblo.up.railway.app/filosofi?x=${x}&y=${y}&radius=${radius}`;

  const response = await fetch(url);
  const data = await response.json();

  return data['message'];
}
```




```js
data 
```


```js
var data = getFilosofi(3756295, 2889313, rayon)
```


