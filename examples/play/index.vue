<template>
  <div style="margin: 20px;">
    <el-input v-model="input" placeholder="请输入内容"></el-input>
    <div :class="['picker']">
      <el-date-picker
        :popper-class="'day-only ams-timeslot-popper'"
        :range-separator="'to'"
        v-model="datetimerange"
        :type="'datetimerange'"
        prefix-icon="d-none"
        :default-time="['12:00:00', '12:00:00']"
        :picker-options="rangeOptions"
        @change="datetimerangeChange"
        :localTime="false"
        :editable="false"
        :clearable="false"
        :unlink-panels="false"
        size="large"
        ref="startpicker"
        :timeVisiable="true"
        >
      </el-date-picker>
    </div>
  </div>
</template>

<script>
  Date.prototype.toCnTimestamp = function(tstamp) {
    return tstamp + new Date(tstamp).getTimezoneOffset() * 60 * 1000 + 8 * 3600 * 1000
  }
  export default {
    props: {
      label: 'test',
      selectedValue: null,
      defaultValue:null,
      multi:{
        type: Boolean,
        default:  function () {
          return true
        }
      },
      autocomplete:{
        type: Boolean,
        default:  function () {
          return true
        }
      },
    },
    data() {
      return {
        input: 'Hello Element UI!',
        filterQuery:'',
        valueArr:[],
        pickerType:'date',
        datetimerange:[1600747200000,1600833600000],
        rangeOptions:{
            disabledDate:(time)=>{
              let td = this.amsDateType().today
              let currentTime = time.getTime() +12*60*60*1000;
              return currentTime > td || currentTime < (td - 92*24*60*60*1000); //92 days = 3 months in backend
            },
        },
      };
    },
    methods: {
      dateChange(d){
        this.startTime = new Date(d).getTime();
        console.log(new Date(this.startTime));
      },
      amsDateType(type = "string") {
        const dayFormat = function(tstamp) {
          let d = new Date(tstamp);
          let output = null;
          if (type == "date") {
            return [d.getUTCFullYear(), (d.getUTCMonth() + 1).padLeft(), d.getUTCDate().padLeft()].join('-') + ' 12:00:00'
          } else {
            return tstamp - (d.getHours() - 12) * 3600 * 1000 - d.getMinutes() * 60 * 1000 - d.getSeconds() * 1000 - d.getMilliseconds() //for cherry
            // return [ d.getUTCFullYear(),(d.getUTCMonth()+1).padLeft(),d.getUTCDate().padLeft()].join('-') + ' ' + 
            //        [ d.getHours().padLeft(),d.getMinutes().padLeft(), d.getSeconds().padLeft() ].join(':')
          }
        }



        // const getDay = function (d) {
        //   return  new Date().getTime() + (1 - (new Date().getDay()!==0?new Date().getDay():7))*dateStamp
        // }

        let dayNow = new Date().toCnTimestamp(new Date().getTime()) //gmt+8

        let hourNow = new Date(dayNow).getHours()

        let dateStamp = 24 * 60 * 60 * 1000;

        //let monthMonday = monthFirstMon(0) //今個月的第一個禮拜一

        //let monthAgoMonday = monthFirstMon(1) //上個月的第一個禮拜一

        let today = dayNow //絕對今天
        let yesterday = dayNow - dateStamp //絕對昨天
        let tomorrow = dayNow + dateStamp //絕對明天

        //let monday = new Date(getDay()) //今個禮拜一
        //let lastMonday = new Date(getDay() - 7*dateStamp) //上個禮拜一

        //對今天中午12點判定
        if (hourNow < 12) {
          today = dayNow - dateStamp
          yesterday = dayNow - 2 * dateStamp //今個禮拜一
          tomorrow = dayNow //今個禮拜一
        }


        return {
          now: type == "date" ? this.$_amsDateFormat(dayNow) : dayNow,
          today: dayFormat(today),
          yesterday: dayFormat(yesterday),
          tomorrow: dayFormat(tomorrow)
          // monday:dayFormat(monday),
          // lastMonday:dayFormat(lastMonday),
          // monthMonday:dayFormat(monthMonday),
          // monthAgoMonday:dayFormat(monthAgoMonday)
        }

      },
      datetimerangeChange(arr){
        console.log(this.datetimerange);
      },
    }
  };
</script>
