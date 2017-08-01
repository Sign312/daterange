# daterange

> 一个基于Vue2.0的日期段选择组件（可在同一界面选择开始，结束时间)

## 用法

熟悉Vue的话直接把代码引入加入钩子函数即可，
示例在/src/App.vue:

```html
        //default_startDate:默认选择的开始日期(Date类型)
        //default_endDate:默认选择的结束日期(Date类型)
        //confirm:选择框点击确认的回调函数(参数：选择的startDate，选择的endDate)
        //cancel:选择框点击取消的回调函数(参数：选择的startDate，选择的endDate)
        <date-range v-model="isShow" :default_startDate="startDate" :default_endDate="endDate" @confirm="confirm"
                    @cancel="cancel">
```

注意:default_startDate和default_endDate要么都不传，要么都传

## demo Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```


