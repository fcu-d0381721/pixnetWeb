<template>
    <div id="map" ref="rootmap"></div>
</template>

<script>

import "ol/ol.css";
import { Map, View } from "ol";
import TileLayer from "ol/layer/Tile";
import OSM from "ol/source/OSM";
import GeoJSON from 'ol/format/GeoJSON';
import VectorSource from "ol/source/Vector";
import VectorLayer from "ol/layer/Vector";
import Style from "ol/style/Style";
import Fill from "ol/style/Fill";
import Stroke from "ol/style/Stroke";
import TileGrid from "ol/tilegrid/TileGrid";
import data from '../assets/100_103_mapcode.json';

export default {
  props: ['btnName'],  
  data() {
    return {
      map: null,
      heatData: data,
      vSource3: null,
    };
  },
  mounted() {
    var mapcontainer = this.$refs.rootmap;

    var osm = new TileLayer({
        source: new OSM()
    });
    this.vSource3 = new VectorSource({
        features: (new GeoJSON()).readFeatures(this.heatData)
    });	
    var vectorLayer3 = new VectorLayer({
        source: this.vSource3,
        style: new Style({
            fill: new Fill({
                color: [255,255,255, 0]
            }),
            stroke: new Stroke({
                color: '#888888',width: 1
            })
        })
    });
    this.map = new Map({
      target: "map",
      layers: [
        osm,vectorLayer3
      ],
      view: new View({
        projection: "EPSG:4326", 
        center: [121, 23.5], 
        zoom: 8
      })
    });
    
  },watch: {
    btnName: function () {
      var features = this.vSource3.getFeatures();
      for (var i = 0;i<features.length;i++) {	
        if(features[i].get("mapcode")==this.btnName){
          var Extent = features[i].getGeometry().getExtent();
          this.map.getView().fit(Extent, this.map.getSize());
          var style = new Style({fill:new Fill({color: [255, 255, 255, 0]}),stroke:new Stroke({color: '#7cb5ec',width: 3})});
          features[i].setStyle(style);
        }else {
          var style = new Style({fill:new Fill({color: [255, 255, 255, 0.5]}),stroke:new Stroke({color: '#888888',width: 1})});
				  features[i].setStyle(style);
        }
      }
    }
  },
};
</script>

<style>
#map{height:100%;}
/*隐藏ol的一些自带元素*/
.ol-attribution,.ol-zoom { display: none;}

</style>