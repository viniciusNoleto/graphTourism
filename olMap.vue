<template>
  <div class="w-full absolute inset-0" id="map"></div>
</template>

<script>

  import Map from 'ol/Map.js';
  import View from 'ol/View.js';
  import Geolocation from 'ol/Geolocation.js';
  import {OSM, Vector as VectorSource} from 'ol/source.js';
  import {Tile as TileLayer, Vector as VectorLayer} from 'ol/layer.js';
  import {Circle as CircleStyle, Fill, Stroke, Icon, Style} from 'ol/style.js';
  import {fromLonLat} from 'ol/proj.js';
  import Point from 'ol/geom/Point.js';
  import Feature from 'ol/Feature.js';

  const view = new View({
    center: [-6755000, 315000],
    zoom: 12,
  });


  // const geolocation = new Geolocation({
  //   trackingOptions: {
  //     enableHighAccuracy: true,
  //   },
  //   projection: view.getProjection(),
  // });

  // geolocation.setTracking(true);

  // const accuracyFeature = new Feature();
  // geolocation.on('change:accuracyGeometry', function () {
  //   accuracyFeature.setGeometry(geolocation.getAccuracyGeometry());
  // });

  // const positionFeature = new Feature();
  // positionFeature.setStyle(
  //   new Style({
  //     image: new CircleStyle({
  //       radius: 6,
  //       fill: new Fill({
  //         color: '#3399CC',
  //       }),
  //       stroke: new Stroke({
  //         color: '#fff',
  //         width: 2,
  //       }),
  //     }),
  //   }),
  // );

  // geolocation.on('change:position', function () {
  //   const coordinates = geolocation.getPosition();
  //   positionFeature.setGeometry(coordinates ? new Point(coordinates) : null);
  // });


  const raster = new TileLayer({
    source: new OSM(),
  });



  const p1 = new Feature({
    geometry: new Point(fromLonLat([-60.6816, 2.8390])),
  });

  p1.setStyle(
    new Style({
      image: new Icon({
        crossOrigin: 'anonymous',
        src: 'public/pin.svg',
        scale: 0.05
      }),
    }),
  );

  p1.setId('Bosque dos Papagaios');


  const p2 = new Feature({
    geometry: new Point(fromLonLat([-60.6580, 2.8593])),
  });

  p2.setStyle(
    new Style({
      image: new Icon({
        crossOrigin: 'anonymous',
        src: 'public/pin.svg',
        scale: 0.05
      }),
    }),
  );

  p2.setId('Parque Anauá');


  const p3 = new Feature({
    geometry: new Point(fromLonLat([-60.6705, 2.8108])),
  });

  p3.setStyle(
    new Style({
      image: new Icon({
        crossOrigin: 'anonymous',
        src: 'public/pin.svg',
        scale: 0.05
      }),
    }),
  );

  p3.setId('Mirante');



  const source = new VectorSource({
    features: [p1, p2, p3],
    wrapX: false,
    useSpatialIndex: true
  });

  const vector = new VectorLayer({
    source: source,
  });

  const map = new Map({
    layers: [raster, vector],
    target: 'map',
    view: view,
  });

  map.on('click', function(event){
    source.getClosestFeatureToCoordinate(event.coordinate).getId();
  });

  export default {
    name: 'Map',
  }

</script>
