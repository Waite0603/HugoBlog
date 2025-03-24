---
title: "Vue 实现公历,农历日期选择效果"
date: 2025-03-22T21:05:54+08:00
lastmod: 2025-03-22T21:05:54+08:00
categories: ["Vue"]
tags: ["Vue"]
author: "Waite Wang"
showToc: true
TocOpen: false
draft: false
hidemeta: false
description: "Vue 学习笔记-系统学习 Pinia"
disableHLJS: true
disableShare: false
hideSummary: false
searchHidden: false
ShowReadingTime: true
ShowBreadCrumbs: true
ShowPostNavLinks: true
ShowWordCount: true
ShowRssButtonInSectionTermList: true
UseHugoToc: true
---



在开发过程中，我们经常需要实现日期选择功能，而有时用户可能希望使用农历日期进行选择。本文将介绍如何使用 Vue 实现一个支持公历和农历选择的日期选择器。

> 注意 这里的农历使用数组，正常来说有其他的方法，但是作者这边不需要。

![image-20250324172736921](http://qiniu.waite.wang/202503241727046.png)

### 实现思路

界面布局：日期选择器分为顶部工具栏和日期选择区域。顶部工具栏包含取消按钮、公历/农历切换按钮和确定按钮。日期选择区域包含年、月、日、时辰（可选）的滚动选择列。

数据管理：使用 Vue 的 ref 和 reactive 来管理选中的日期和日历类型。通过 computed 属性动态生成年、月、日、时辰的选项。

滚动交互：通过监听滚动事件，实时更新选中的日期，并实现平滑滚动效果。

农历支持：通过数组存储农历月份和日期的名称，根据用户选择的日历类型动态显示公历或农历。

```vue
<template>
	<div class="date-picker">
		<!-- 顶部工具栏 -->
		<div class="picker-header">
			<div class="picker-toolbar">
				<span class="picker-cancel" @click="onCancel">取消</span>
				<div class="picker-tabs">
					<div
						v-for="tab in ['公历', '农历']"
						:key="tab"
						:class="['picker-tab', { active: calendarType === tab }]"
						@click="calendarType = tab"
					>
						{{ tab }}
					</div>
				</div>
				<span class="picker-confirm" @click="onConfirm">确定</span>
			</div>
		</div>

		<!-- 日期选择区域 -->
		<div class="date-picker-content">
			<div class="date-columns">
				<div class="date-column" ref="yearColumn">
					<div
						class="date-column-items"
						@scroll="onColumnScroll($event, 'year')"
					>
						<div
							v-for="year in yearOptions"
							:key="year.value"
							:class="[
								'date-item',
								{ active: selectedDate.year === year.value }
							]"
							@click="selectYear(year.value)"
						>
							{{ year.text }}
						</div>
					</div>
				</div>
				<div class="date-column" ref="monthColumn">
					<div
						class="date-column-items"
						@scroll="onColumnScroll($event, 'month')"
					>
						<div
							v-for="month in monthOptions"
							:key="month.value"
							:class="[
								'date-item',
								{ active: selectedDate.month === month.value }
							]"
						>
							{{ month.text }}
						</div>
					</div>
				</div>
				<div class="date-column" ref="dayColumn">
					<div
						class="date-column-items"
						@scroll="onColumnScroll($event, 'day')"
					>
						<div
							v-for="day in dayOptions"
							:key="day.value"
							:class="['date-item', { active: selectedDate.day === day.value }]"
						>
							{{ day.text }}
						</div>
					</div>
				</div>
				<div class="date-column" ref="hourColumn" v-if="showHour">
					<div
						class="date-column-items"
						@scroll="onColumnScroll($event, 'hour')"
					>
						<div
							v-for="hour in hourOptionsComputed"
							:key="hour.value"
							:class="[
								'date-item',
								{ active: selectedDate.hour === hour.value }
							]"
						>
							{{ hour.text }}
						</div>
					</div>
				</div>
			</div>
			<!-- 选中区域指示器 -->
			<div class="date-picker-indicator"></div>
		</div>
	</div>
</template>

<script setup>
import { ref, reactive, computed, watch, nextTick, onMounted } from 'vue'
import { lunarMonths, lunarDays, hourOptions } from '@/constants/date'

const props = defineProps({
	modelValue: {
		type: Date,
		default: () => new Date()
	},
	minDate: {
		type: Date,
		default: () => new Date(1900, 0, 1)
	},
	maxDate: {
		type: Date,
		default: () => new Date()
	},
	showHour: {
		type: Boolean,
		default: false
	},
	initialHour: {
		type: String,
		default: ''
	}
})

const emit = defineEmits(['update:modelValue', 'confirm', 'cancel'])

// 日历类型
const calendarType = ref('公历')

// 列引用
const yearColumn = ref(null)
const monthColumn = ref(null)
const dayColumn = ref(null)
const hourColumn = ref(null)

// 当前选中的日期
const selectedDate = reactive({
	year: props.modelValue.getFullYear(),
	month: props.modelValue.getMonth() + 1,
	day: props.modelValue.getDate(),
	hour: props.modelValue.hour || hourOptions[0].value
})

// 年份选项
const yearOptions = computed(() => {
	const years = []
	const startYear = 1900
	const endYear = new Date().getFullYear()
	for (let i = startYear; i <= endYear; i++) {
		years.push({ text: `${i}年`, value: i })
	}
	return years
})

// 修改月份选项的计算属性
const monthOptions = computed(() => {
	const months = []
	for (let i = 1; i <= 12; i++) {
		const month = String(i).padStart(2, '0')
		months.push({
			text: calendarType.value === '农历' ? lunarMonths[i - 1] : `${month}月`,
			value: i
		})
	}
	return months
})

// 修改日期选项的计算属性
const dayOptions = computed(() => {
	const days = []
	const daysInMonth = new Date(
		selectedDate.year,
		selectedDate.month,
		0
	).getDate()
	for (let i = 1; i <= daysInMonth; i++) {
		const day = String(i).padStart(2, '0')
		days.push({
			text: calendarType.value === '农历' ? lunarDays[i - 1] : `${day}日`,
			value: i
		})
	}
	return days
})

// 修改时辰选项为直接使用导入的常量
const hourOptionsComputed = computed(() => hourOptions)

// 添加防抖函数
const debounce = (fn, delay) => {
	let timer = null
	return function (...args) {
		if (timer) clearTimeout(timer)
		timer = setTimeout(() => {
			fn.apply(this, args)
		}, delay)
	}
}

// 处理列滚动
const onColumnScroll = debounce((event, type) => {
	const el = event.target
	const scrollTop = el.scrollTop
	const itemHeight = 44

	// 计算最接近的选项索引
	const index = Math.round(scrollTop / itemHeight)

	// 获取对应的选项列表
	let options
	switch (type) {
		case 'year':
			options = yearOptions.value
			break
		case 'month':
			options = monthOptions.value
			break
		case 'day':
			options = dayOptions.value
			break
		case 'hour':
			options = hourOptionsComputed.value
			console.log(options)
			break
	}

	// 确保索引在有效范围内
	if (index >= 0 && index < options.length) {
		// 更新选中值
		const value = options[index].value
		switch (type) {
			case 'year':
				selectedDate.year = value
				break
			case 'month':
				selectedDate.month = value
				break
			case 'day':
				selectedDate.day = value
				break
			case 'hour':
				selectedDate.hour = value
				console.log(selectedDate.hour)
				break
		}

		// 平滑滚动到正确位置
		el.scrollTo({
			top: index * itemHeight,
			behavior: 'smooth'
		})
	}
}, 100)

// 修改滚动到选中项的方法
const scrollToSelected = () => {
	nextTick(() => {
		const columns = {
			year: yearColumn.value,
			month: monthColumn.value,
			day: dayColumn.value,
			hour: hourColumn.value
		}

		Object.entries(columns).forEach(([type, column]) => {
			if (!column || (type === 'hour' && !props.showHour)) return

			const itemsEl = column.querySelector('.date-column-items')
			const options = {
				year: yearOptions.value,
				month: monthOptions.value,
				day: dayOptions.value,
				hour: hourOptionsComputed.value
			}[type]

			const index = options.findIndex(
				(item) => item.value === selectedDate[type]
			)
			if (index >= 0) {
				itemsEl.scrollTo({
					top: index * 44,
					behavior: 'smooth'
				})
			}
		})
	})
}

// 确认选择
const onConfirm = () => {
	const date = new Date(
		selectedDate.year,
		selectedDate.month - 1,
		selectedDate.day
	)

	emit('update:modelValue', date)
	emit('confirm', {
		date,
		calendarType: calendarType.value,
		hour: selectedDate.hour
	})
}

// 取消选择
const onCancel = () => {
	emit('cancel')
}

// 组件挂载后初始化
onMounted(() => {
	nextTick(() => {
		if (props.initialHour) {
			const hourOption = hourOptions.find((h) => h.text === props.initialHour)
			if (hourOption) {
				selectedDate.hour = hourOption.value
			}
		}
		scrollToSelected()
	})
})

// 监听日历类型变化，更新显示
watch(calendarType, () => {
	nextTick(() => {
		scrollToSelected()
	})
})
</script>

<style scoped>
.date-picker {
	background-color: #fff;
	user-select: none;
}

.picker-header {
	background: #f7f8fa;
}

.picker-toolbar {
	display: flex;
	align-items: center;
	padding: 10px 16px;
	border-bottom: 1px solid #ebedf0;
}

.picker-cancel,
.picker-confirm {
	flex: 1;
	font-size: 14px;
	color: #969799;
	cursor: pointer;
}

.picker-confirm {
	text-align: right;
	color: #8b4513;
}

.picker-tabs {
	flex: 2;
	display: flex;
	justify-content: center;
}

.picker-tab {
	padding: 8px 16px;
	font-size: 14px;
	color: #969799;
	position: relative;
	cursor: pointer;
}

.picker-tab.active {
	color: #8b4513;
	font-weight: bold;
}

.picker-tab.active::after {
	content: '';
	position: absolute;
	bottom: -11px;
	left: 50%;
	transform: translateX(-50%);
	width: 20px;
	height: 2px;
	background-color: #8b4513;
}

.date-picker-content {
	height: 220px;
	position: relative;
	overflow: hidden;
	padding: 20px 16px;
}

.date-columns {
	display: flex;
	height: 100%;
}

.date-column {
	height: 100%;
	overflow: hidden;
	text-align: center;
	background-color: #fff;
	margin: 0 2px;
}

/* 添加宽度比例控制 */
.date-column:nth-child(1),
.date-column:nth-child(2),
.date-column:nth-child(3) {
	flex: 0 0 20%; /* 前三列各占 20% */
}

.date-column:nth-child(4) {
	flex: 0 0 40%; /* 最后一列占 40% */
}

.date-column-items {
	height: 100%;
	overflow-y: auto;
	-webkit-overflow-scrolling: touch;
	padding: 88px 0;
	scroll-snap-type: y mandatory;
}

/* 隐藏滚动条但保留功能 */
.date-column-items::-webkit-scrollbar {
	display: none;
}

.date-column-items {
	-ms-overflow-style: none;
	scrollbar-width: none;
}

.date-item {
	height: 44px;
	line-height: 44px;
	font-size: 14px;
	color: #999;
	cursor: pointer;
	scroll-snap-align: center;
	opacity: 0.6;
	transition: all 0.3s;
}

.date-item.active {
	color: #8b4513;
	font-weight: bold;
	opacity: 1;
	font-size: 16px;
}

/* 移除中间选中区域的渐变效果 */
.date-picker-content::before,
.date-picker-content::after {
	display: none;
}

/* 移除选择条 */
.date-picker-indicator {
	display: none;
}
</style>
```

```javascript
// 农历月份
export const lunarMonths = [
  '正月', '二月', '三月', '四月', '五月', '六月',
  '七月', '八月', '九月', '十月', '冬月', '腊月'
]

// 农历日期
export const lunarDays = [
  '初一', '初二', '初三', '初四', '初五', '初六', '初七', '初八', '初九', '初十',
  '十一', '十二', '十三', '十四', '十五', '十六', '十七', '十八', '十九', '二十',
  '廿一', '廿二', '廿三', '廿四', '廿五', '廿六', '廿七', '廿八', '廿九', '三十'
]
```

