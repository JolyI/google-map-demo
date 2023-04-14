<script setup lang="ts">
import { IndoesiaPoint } from '@/global/const'
import { watchEffect } from 'vue'
import type { Points } from './interface'
const iconUrl = '//static.xyb2b.com/images/a666c7fd32b9cf97848f1d271de0f1bf.svg'
let map: any
let markers: google.maps.Marker[] = []
let infowindow: google.maps.InfoWindow
let polylines: google.maps.Polyline = []
let polyons: google.maps.Polygon = []

// 模拟多段线
const polylinePaths: Points[] = [
  { lat: -7.101935050883591, lng: 107.4588552036972 },
  { lat: -6.694838728520297, lng: 107.75462324500616 },
  { lat: -6.847630347242984, lng: 108.19105762732289 },
  { lat: -7.178153561208554, lng: 108.13234630024549 },
  { lat: -7.5499689900116005, lng: 107.909873169182 },
  { lat: -6.64506887158396, lng: 106.31099036415657 },
  { lat: -8.021581922136363, lng: 108.26073130399932 }
]

const initMap = async () => {
  const center = IndoesiaPoint
  // @ts-ignore
  const { Map } = await google.maps.importLibrary('maps')
  map = new Map(document.getElementById('map'), {
    zoom: 10,
    center,
    mapTypeControl: true,
    zoomControl: true
  })

  // @ts-ignore
  const { InfoWindow } = await google.maps.importLibrary('maps')
  infowindow = new InfoWindow()
  // 加载marker
  // initMarker()
  // // 多段线Polyline 类
  // initPolyline()
  // polygon类 闭合线
  initPolygon()
}

const initMarker = async () => {
  deleteMarker()
  // @ts-ignore
  const { Marker } = await google.maps.importLibrary('marker')
  // 可以循环加载多个
  const marker = new Marker({
    optimized: true,
    position: { lat: -6.905924, lng: 107.59888 },
    title: `印尼唐格朗`,
    icon: iconUrl,
    map
  })

  marker.addListener('click', () => {
    infowindow.setContent('loading...')
    infowindow?.open({
      anchor: marker,
      map: map
    })
    let contentStr = '这是我想放置的数据' // 这里可以放你想显示的任何数据
    infowindow.setContent(contentStr)
  })

  markers.push(marker)
}

// 删除marker
const deleteMarker = () => {
  markers?.forEach((m) => {
    m?.setMap(null)
  })
}

// 批量设置marker
const setMarker = async (position: Points) => {
  // @ts-ignore
  const { Marker } = await google.maps.importLibrary('marker')
  // 可以循环加载多个
  const marker = new Marker({
    optimized: true,
    position: position,
    title: `${Object.values(position).join(',')}`,
    icon: iconUrl,
    map
  })

  marker.addListener('click', () => {
    infowindow.setContent('loading...')
    infowindow?.open({
      anchor: marker,
      map: map
    })
    let contentStr = '这是我想放置的数据' // 这里可以放你想显示的任何数据
    infowindow.setContent(contentStr)
  })

  markers.push(marker)
}

// 多段线
const initPolyline = async () => {
  //@ts-ignore
  const { Polyline } = await google.maps.importLibrary('maps')
  const polyline = new Polyline({
    map,
    path: polylinePaths,
    strokeColor: '#FF6801',
    strokeOpacity: 0.9,
    strokeWeight: 3,
    clickable: false
  })
  polylines.push(polyline)

  // 可以配合 设置marker 让路线更具体
  polylinePaths.forEach((position: Points) => {
    setMarker(position)
  })
}

// polygon类 闭合线
const initPolygon = async () => {
  // @ts-ignore
  const { Polygon } = await google.maps.importLibrary('maps')
  const polygon = new Polygon({
    path: polylinePaths,
    strokeColor: '#FF6801',
    strokeOpacity: 0.9,
    strokeWeight: 3,
    clickable: false,
    fillColor: '#FF6801',
    map
  })
  polyons.push(polygon)
}

watchEffect(() => {
  console.log('watchEffect')
  initMap()
})
</script>
<template>
  <div class="page-map">
    <div id="map"></div>
  </div>
</template>
<style scoped lang="less">
.page-map {
  height: 100vh;
  width: 100vw;
}
#map {
  /* The height is 400 pixels */
  width: 100%;
  height: 100%;
  /* The width is the width of the web page */
  border-radius: 10px;
  #content {
    color: red;
  }
}
</style>
