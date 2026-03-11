# Hubblo

Connaitre la population autour de vous

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
async function getFilosofi(x, y, radius) {
  const url = `https://hubblo.up.railway.app/filosofi?x=${x}&y=${y}&radius=${radius}`;

  const response = await fetch(url);
  const data = await response.json();

  return data['message'];
}
```




```js
getFilosofi(3756295, 2889313, rayon)
```


