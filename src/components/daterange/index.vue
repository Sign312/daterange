<template>
    <div v-show="isShow" class="date-range-wrapper">
        <div class="date-range-shadow"></div>
        <div class="date-range-box">
            <div class="box-body">
                <div class="date-range-calendar">
                    <div class="date-range-display">
                        <div class="start">{{startDateString}}</div>
                        <div class="to">到</div>
                        <div class="end">{{endDateString}}</div>
                    </div>
                    <div class="calendar-container">
                        <div class="main">
                            <div class="tool-bar">
                                <div class="arrow" @click="toLastMonth">&lt;</div>
                                <div class="display">{{monthPointDateString}}</div>
                                <div class="arrow" @click="toNextMonth">&gt;</div>
                            </div>
                            <div class="weeks">
                                <span class="week">一</span>
                                <span class="week">二</span>
                                <span class="week">三</span>
                                <span class="week">四</span>
                                <span class="week">五</span>
                                <span class="week">六</span>
                                <span class="week">日</span>
                            </div>
                            <div class="date-box">
                                <div class="date" :class="pick.customerClass" v-for="pick in picks"
                                     @click="pickDate(pick.date)">
                                    {{pick.displayNumber}}
                                </div>
                            </div>
                        </div>
                        <div class="confirm-box">
                            <div class="cancel" @click="cancel">取消</div>
                            <div class="confirm" @click="confirm">确定</div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<style lang="scss">
    @import "./style.scss";
</style>
<script>
	export default{
		props: ['value', 'default_startDate', 'default_endDate'],
		data () {
			return {
				startDate: this.default_startDate ? this.default_startDate : new Date(),
				endDate: this.default_endDate ? this.default_endDate : new Date(new Date().setDate(new Date().getDate() + 10)),
				monthPointDate: this.default_startDate ? this.default_startDate : new Date(),
				picks: []
			}
		},
		methods: {
			confirm () {
				this.$emit('confirm', this.startDate, this.endDate)
			},
			cancel () {
				this.$emit('cancel', this.startDate, this.endDate)
			},
			pickDate(date){
				let monthPointDate = this.monthPointDate
				let startDate = this.startDate
				let endDate = this.endDate
				if (date.getMonth() != monthPointDate.getMonth()) {
					return
				}
				if (date.toDateString() == startDate.toDateString() || date.toDateString() == endDate.toDateString()) {
					if (date.toDateString() == startDate.toDateString()) {
						this.endDate = date
					} else {
						this.startDate = date
					}
				} else {
					if (date.getTime() > endDate.getTime()) {
						this.endDate = date
					} else if (date.getTime() < startDate.getTime()) {
						this.startDate = date
					} else {
						let startDistance = date.getTime() - startDate.getTime()
						let endDistance = endDate.getTime() - date.getTime()
						if (startDistance >= endDistance) {
							this.endDate = date
						} else {
							this.startDate = date
						}
					}
				}
				this.createCalendar(this.monthPointDate)
			},
			createPickByDate (date) {
				let customerClass = ''
				let monthPointDate = this.monthPointDate
				let startDate = this.startDate
				let endDate = this.endDate
				if (date.getMonth() != monthPointDate.getMonth()) {
					customerClass = 'light'
				} else {
					if (date.toDateString() == startDate.toDateString() || date.toDateString() == endDate.toDateString()) {
						customerClass = 'start'
					} else {
						if (date.getTime() > startDate.getTime() && date.getTime() < endDate.getTime()) {
							customerClass = 'pass'
						}
					}
				}
				return {
					date: date,
					displayNumber: date.getDate(),
					customerClass: customerClass
				}
			},
			toLastMonth () {
				let tempDate = new Date(this.monthPointDate)
				tempDate.setMonth(tempDate.getMonth() - 1)
				this.monthPointDate = tempDate
				this.createCalendar(this.monthPointDate)
			},
			toNextMonth () {
				let tempDate = new Date(this.monthPointDate)
				tempDate.setMonth(tempDate.getMonth() + 1)
				this.monthPointDate = tempDate
				this.createCalendar(this.monthPointDate)
			},
			createCalendar (monthPointDate) {

				this.picks = []

				let picks = []
				let monthStartDate = new Date(monthPointDate.getFullYear(), monthPointDate.getMonth(), 1)

				//要显示的上月的dates
				let lastMonthRestNum = monthStartDate.getDay() == 0 ? 6 : monthStartDate.getDay() - 1
				for (let i = lastMonthRestNum; i > 0; --i) {
					let tempDate = new Date(monthStartDate)
					tempDate.setDate(tempDate.getDate() - i)
					picks.push(this.createPickByDate(tempDate))
				}
				//本月dates
				let tempIndexDate = new Date(monthStartDate)
				while (tempIndexDate.getMonth() === monthStartDate.getMonth()) {
					let tempDate = new Date(tempIndexDate)
					picks.push(this.createPickByDate(tempDate))
					tempIndexDate.setDate(tempIndexDate.getDate() + 1)
				}
				//要显示的下月的dates
				while (tempIndexDate.getDay() != 1) {
					let tempDate = new Date(tempIndexDate)
					picks.push(this.createPickByDate(tempDate))
					tempIndexDate.setDate(tempIndexDate.getDate() + 1)
				}

				this.picks = picks
			}
		},
		computed: {
			isShow () {
				return this.value
			},
			startDateString () {
				let startDate = this.startDate
				return startDate.getFullYear() + '-' + (startDate.getMonth() + 1) + '-' + startDate.getDate()
			},
			endDateString () {
				let endDate = this.endDate
				return endDate.getFullYear() + '-' + (endDate.getMonth() + 1) + '-' + endDate.getDate()
			},
			monthPointDateString () {
				let monthPointDate = this.monthPointDate
				return monthPointDate.getFullYear() + '年' + ( monthPointDate.getMonth() + 1) + '月'
			}
		},
		created () {
			this.createCalendar(this.monthPointDate)
		}
	}
</script>
