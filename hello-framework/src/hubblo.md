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
  .setView([51.505, -0.09], 13);

L.tileLayer("https://tile.openstreetmap.org/{z}/{x}/{y}.png", {
  attribution: '&copy; <a href="https://www.openstreetmap.org/copyright">OpenStreetMap</a>'
})
  .addTo(map);

L.marker([51.5, -0.09])
  .addTo(map)
  .bindPopup("A nice popup<br> indicating a point of interest.")
  .openPopup();
```

```js echo
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

    const valueCell = document.createElement("td");
    valueCell.textContent = value[0];

    row.appendChild(nameCell);
    row.appendChild(valueCell);
    table.appendChild(row);
  }

  return table;
}
```

```js
coordinates3035 = proj4('+proj=longlat +datum=WGS84 +no_defs +type=crs',
                        '+proj=laea +lat_0=52 +lon_0=10 +x_0=4321000 +y_0=3210000 +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +units=m +no_defs +type=crs',
                        [newYourMap.lng, newYourMap.lat]);
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
LeafletMap = (value = { lat: 43.596, lng: 1.4419 }) => {
  const { lat, lng } = value;
  const curValue = { lat, lng };
  const store = { value: curValue };

  const container = html`<div style="height:400px;">`;
  const map = L.map(container).setView([lat, lng], 10);
  L.tileLayer("https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png", { attribution: '© OpenStreetMap' }).addTo(map);
  const placeIcon = L.icon({
    iconUrl: 'https://cdn.observableusercontent.com/npm/leaflet@1.8.0/dist/images/marker-icon.png',
    shadowUrl: 'https://cdn.observableusercontent.com/npm/leaflet@1.8.0/dist/images/marker-shadow.png',
    iconAnchor: [12, 41]
  });
  const marker = L.marker([store.value.lat, store.value.lng], { alt: 'Point', icon: placeIcon });
  marker.addTo(map);

  function onMapClick(e) {      
    store.value = e.latlng;
    marker.setLatLng(e.latlng);
    container.dispatchEvent(new Event("input", { bubbles: true })); // Native events bubble, so we should too
  }
  map.on('click', onMapClick);

  // Update the display whenever the value changes
  Object.defineProperty(container, "value", {
    get() {
      return store.value;
    },
    set(v) {
      store.value = v;
    }
  });

  // Set the initial value
  container.value = curValue;

  return container;
}
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
