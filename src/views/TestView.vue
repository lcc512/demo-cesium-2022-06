<template>
  <div class="container">
    <div id="cesium-container"></div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      viewer: null,
    }
  },
  mounted() {
    this.initCesium()
  },

  methods: {
    initCesium() {
      Cesium.Ion.defaultAccessToken =
        'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJqdGkiOiIwYTkwYjYwOC01ZmNkLTRmNTAtODdjMC00ODg1YTBiYWZiZjIiLCJpZCI6OTg3MTQsImlhdCI6MTY1NTk2MDgxNH0.q3IsC836AJ9JaIuARbtoTsx10MVCUpfg-xj7finea3I'
      this.viewer = new Cesium.Viewer('cesium-container', {
        scene3DOnly: true,
        selectionIndicator: false,
        baseLayerPicker: false,
        // 时间动画
        // animation: false,
        // timeline: false,
        // fullscreenButton: false,
        // 地址搜索
        // geocoder: false,
      })

      // 3D地形 Grand Canyon
      // this.viewer.terrainProvider = Cesium.createWorldTerrain({
      //   requestWaterMask: true, // required for water effects
      //   requestVertexNormals: true, // required for terrain lighting
      // })
      // this.viewer.scene.globe.depthTestAgainstTerrain = true

      // Enable lighting based on sun/moon positions
      // 光照跟随太阳月亮位置改变
      // this.viewer.scene.globe.enableLighting = true

      setTimeout(() => {
        this.initCamera()
      }, 4000)
    },

    // 摄像机位置
    initCamera() {
      // Create an initial camera view
      var initialPosition = new Cesium.Cartesian3.fromDegrees(
        -73.998114468289017509,
        40.674512895646692812,
        2631.082799425431
      )
      var initialOrientation = new Cesium.HeadingPitchRoll.fromDegrees(
        7.1077496389876024807,
        -31.987223091598949054,
        0.025883251314954971306
      )
      var homeCameraView = {
        destination: initialPosition,
        orientation: {
          heading: initialOrientation.heading,
          pitch: initialOrientation.pitch,
          roll: initialOrientation.roll,
        },
      }

      // Set the initial view
      // this.viewer.scene.camera.setView(homeCameraView)

      var wyoming = this.viewer.entities.add({
        name: 'Wyoming',
        polygon: {
          hierarchy: Cesium.Cartesian3.fromDegreesArray([
            -109.080842, 45.002073, -105.91517, 45.002073, -104.058488,
            44.996596, -104.053011, 43.002989, -104.053011, 41.003906,
            -105.728954, 40.998429, -107.919731, 41.003906, -109.04798,
            40.998429, -111.047063, 40.998429, -111.047063, 42.000709,
            -111.047063, 44.476286, -111.05254, 45.002073,
          ]),
          height: 0,
          material: Cesium.Color.RED.withAlpha(0.5),
          outline: true,
          outlineColor: Cesium.Color.BLACK,
        },
      })

      wyoming.polygon.height = 200000
      wyoming.polygon.extrudedHeight = 250000

      var entity = this.viewer.entities.add({
        position: Cesium.Cartesian3.fromDegrees(-103.0, 40.0),
        ellipse: {
          semiMinorAxis: 250000.0,
          semiMajorAxis: 400000.0,
          material: '/lib/images/caizhi.jpg',
        },
      })
      this.viewer.zoomTo(this.viewer.entities)

      // this.viewer.zoomTo(wyoming)
    },
  },
}
</script>

<style lang="scss" scoped>
.container {
  width: 100vw;
  height: 100vh;
  #cesium-container {
    width: 100vw;
    height: 100vh;
  }
}

// 隐藏地图底部的控件
::v-deep .cesium-viewer-bottom {
  display: none;
}
</style>
