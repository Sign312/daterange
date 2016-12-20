<template>
    <div id="app">
        start:<input  v-model="startDateString" @click.prevent="test" onfocus="this.blur()">
        <br>
        end:<input  v-model="endDateString" @click.prevent="test" onfocus="this.blur()">
        <date-range v-model="isShow" :default_startDate="startDate" :default_endDate="endDate" @confirm="confirm"
                    @cancel="cancel">
        </date-range>
    </div>
</template>
<script>
	import dateRange from './components/daterange'
	export default {
		name: 'app',
		data () {
			return {
				isShow: false,
				startDate: new Date(),
				endDate: new Date()
			}
		},
		components: {
			dateRange
		},
		methods: {
			test () {
				this.isShow = true
			},
			confirm (startDate, endDate) {
				console.log('confirm:')
				console.log('startDate is ' + startDate)
				console.log('endDate is ' + endDate)
				this.startDate = startDate
				this.endDate = endDate
				this.isShow = false
			},
			cancel (startDate, endDate) {
				console.log('cancel:')
				console.log('startDate is ' + startDate)
				console.log('endDate is ' + endDate)
				this.isShow = false
			}
		},
		created () {
			let tempDate = this.endDate
			tempDate.setDate(tempDate.getDate() + 10)
			this.endDate = tempDate
		},
		computed: {
			startDateString () {
				let startDate = this.startDate
				return startDate.getFullYear() + '-' + (startDate.getMonth() + 1) + '-' + startDate.getDate()
			},
			endDateString () {
				let endDate = this.endDate
				return endDate.getFullYear() + '-' + (endDate.getMonth() + 1) + '-' + endDate.getDate()
			},
		}
	}
</script>
<style>
    html, body {
        margin: 0;
        padding: 0;
    }
</style>
