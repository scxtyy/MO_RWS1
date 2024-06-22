<script lang="ts">
    import { onMount } from 'svelte';
    import mapboxgl from 'mapbox-gl';
    import geojson from './Path'; // Remove the .ts extension

    let mapContainer: HTMLDivElement | null = null;
    let getal1: number = 0; // Afvoer
    let getal2: number = 0; // Waterhoogte
    let getal3: number = 0; // Watertemperatuur

    // De lijst met getallen voor Waterhoogte, Afvoer en Watertemperatuur
    const waterhoogteList = [
        4.454, 4.406, 4.394, 3.870, 3.790, 3.601, 3.436, 2.884, 2.724, 2.600,
        2.244, 2.120, 2.096, 1.429, 1.423, 1.408, 1.157, 1.149, 1.120, 1.109,
        0.809, 0.795, 0.785, 0.781, 0.511, 0.496, 0.490
    ];
    const afvoerList = [
        239.04, 132.54, 301.87, 314.99, 388.83
    ];
    const watertemperatuurList = [
        18.5, 18.5, 18.8, 18.6, 18.8, 18.7, 18.8, 18.6, 18.2, 18.6, 
        18.8, 18.7, 18.8, 18.6, 18.2
    ];
    let waterhoogteIndex = 0;
    let afvoerIndex = 0;
    let watertemperatuurIndex = 0;

    function updateNumbers() {
        if (waterhoogteIndex < waterhoogteList.length) {
            getal2 = waterhoogteList[waterhoogteIndex]; // Waterhoogte
            waterhoogteIndex++;
        }
        if (afvoerIndex < afvoerList.length) {
            getal1 = afvoerList[afvoerIndex]; // Afvoer
            afvoerIndex++;
        }
        if (watertemperatuurIndex < watertemperatuurList.length) {
            getal3 = watertemperatuurList[watertemperatuurIndex]; // Watertemperatuur
            watertemperatuurIndex++;
        }
    }

    onMount(() => {
        mapboxgl.accessToken = 'pk.eyJ1Ijoic2hhZG9uMTU2IiwiYSI6ImNseDRnY3pxczBkdWQyanF0bm14ZHVvbGkifQ.SU1EtXJUTlEVINngdmWkfA';
        if (mapContainer) {
            const map = new mapboxgl.Map({
                container: mapContainer,
                style: 'mapbox://styles/shadon156/clx4mk5hp01td01pn5n8r2qba', // style URL
                center: [5.684197812691798, 50.76291740322954], // starting position [lng, lat]
                zoom: 18, // starting zoom
                pitch: 60, // tilt the map
                bearing: -17.6 // rotate the map
            });

            map.on('load', () => {
                // Add the GeoJSON data to the map
                map.addSource('path', {
                    'type': 'geojson',
                    'data': geojson
                });

                map.addLayer({
                    'id': 'path',
                    'type': 'line',
                    'source': 'path',
                    'layout': {
                        'line-join': 'round',
                        'line-cap': 'round'
                    },
                    'paint': {
                        'line-color': '#888',
                        'line-width': 8
                    }
                });

                // Add 3D buildings
                map.addLayer({
                    'id': '3d-buildings',
                    'source': 'composite',
                    'source-layer': 'building',
                    'filter': ['==', 'extrude', 'true'],
                    'type': 'fill-extrusion',
                    'minzoom': 15,
                    'paint': {
                        'fill-extrusion-color': '#aaa',
                        'fill-extrusion-height': [
                            'interpolate',
                            ['linear'],
                            ['zoom'],
                            15,
                            0,
                            15.05,
                            ['get', 'height']
                        ],
                        'fill-extrusion-base': [
                            'interpolate',
                            ['linear'],
                            ['zoom'],
                            15,
                            0,
                            15.05,
                            ['get', 'min_height']
                        ],
                        'fill-extrusion-opacity': 1
                    }
                });

                // Create a marker
                const marker = new mapboxgl.Marker()
                    .setLngLat(geojson.features[0].geometry.coordinates[0])
                    .addTo(map);

                // Function to animate the marker along the path
                let step = 0;
                const coordinates = geojson.features[0].geometry.coordinates;
                const animationSpeed = 5000; // Increase the duration to slow down the animation

                function animateMarker() {
                    const start = coordinates[step];
                    const end = coordinates[step + 1];
                    if (!start || !end) return;

                    // Update the marker position
                    marker.setLngLat(end);
                    updateNumbers(); // Update the numbers when reaching a new point

                    step += 1;
                    if (step < coordinates.length - 1) {
                        setTimeout(animateMarker, animationSpeed); // Add delay between steps
                    }
                }

                animateMarker();

                // Fit the map to the bounds of the GeoJSON
                const bounds = new mapboxgl.LngLatBounds();
                geojson.features.forEach(feature => {
                    feature.geometry.coordinates.forEach(coord => {
                        bounds.extend(coord);
                    });
                });
                map.fitBounds(bounds, {
                    padding: 20
                });

                // Optionally, animate the map to follow the path
                let mapStep = 0;

                function animateMap() {
                    const start = coordinates[mapStep >= coordinates.length ? mapStep - 1 : mapStep];
                    const end = coordinates[mapStep >= coordinates.length - 1 ? mapStep : mapStep + 1];
                    if (!start || !end) return;

                    // Update the camera position to be between start and end
                    map.easeTo({
                        center: end,
                        duration: animationSpeed,
                        easing: (t: number) => t
                    });

                    mapStep += 1;
                    if (mapStep < coordinates.length) {
                        setTimeout(animateMap, animationSpeed); // Add delay between steps
                    }
                }

                animateMap();
            });
        }
    });
</script>

<div bind:this={mapContainer} style="width: 100%; height: 100vh;"></div>
<a class="absolute top-2 left-2 z-10 bg-accent px-8 py-3 font-bold rounded-xl" href="/KiesJeBoot">terug</a>
<div class="overlay">
    <p>Afvoer: {getal1} m3/s</p>
    <p>Waterhoogte: {getal2} cm</p>
    <p>Watertemperatuur: {getal3} °C</p>
</div>

<style>
    .overlay {
        position: absolute;
        bottom: 20px;
        right: 20px;
        background-color: rgba(255, 255, 255, 0.8);
        padding: 10px;
        border-radius: 8px;
        box-shadow: 0 2px 10px rgba(0, 0, 0, 0.3);
        font-size: 18px;
        width: 250px; /* Fixed width for the overlay box */
    }
</style>
