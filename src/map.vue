<template lang="jade">
.map
</template>
<script>
import leaflet from 'leaflet'
import 'leaflet/dist/leaflet.css'
import config from 'config'
import api from './feinstaub-api'
import './hexbin-layer'

export default {
	mounted () {
		this.$nextTick(() => {
			let map = leaflet.map(this.$el, {
				center: config.center,
				zoom: 11
			})
			leaflet.tileLayer('http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
				attribution: 'Map data © <a href="http://openstreetmap.org">OpenStreetMap</a> contributors',
				maxZoom: 13,
				// continuousWorld: false,
				// noWrap: true

			}).addTo(map)

			let options = {
				mouseover: (data) => {
					this.$emit('cell-selected', data)
				},
				mouseout: () => {
				},
				click: (data) => {
					this.$emit('cell-selected', data)
				}
			}

			let hexLayer = new leaflet.HexbinLayer(options).addTo(map)

			api.getAllSensors().then((cells) => {
				hexLayer.data(cells)
			})
		})
	}
}
</script>
<style lang="stylus">
</style>
