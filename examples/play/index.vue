<template>
  <div class="day-range" style="margin: 20px;">
    <el-input v-model="input" placeholder="请输入内容"></el-input>
    <div :class="['picker']">
      <el-date-picker
        :popper-class="'hour-only ams-timeslot-popper'"
        :range-separator="'to'"
        v-model="datetimerange"
        :type="'datetimerange'"
        prefix-icon="d-none"
        format="yyyy-MM-dd HH:mm"
        :default-time="['12:00:00', '12:00:00']"
        :picker-options="rangeOptions"
        @change="datetimerangeChange"
        :timeVisiable="true"
        :localTime="false"
        :editable="false"
        :clearable="false"
        :unlink-panels="false"
        size="large"
        ref="datetimerange"
        :timeReadOnly="false"
        >
        <div slot="quickSearch">asdasdasd,asdasda</div>
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
      firstMonday:null,
      datePickOptions:null
    },
    data() {
      return {
        input: 'Hello Element UI!',
        filterQuery:'',
        valueArr:[],
        pickerType:'date',
        datetimerange:[this.amsDateType().today,this.amsDateType().tomorrow],
        td:this.amsDateType().today,
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
<style lang="scss">
@import '../assets/styles/common.css';
.el-picker-panel.el-popper{
  z-index: 10!important;
}
.ams-timeslot-popper{
  .el-scrollbar__wrap{
    box-sizing: content-box;
  }
  &.day-only{
    .el-date-range-picker__editor{
      input.el-input__inner{
        background-color: #f5f7fa;
        border-color: #e4e7ed;
        color: #c0c4cc;
      }
      &::before {
        display: block;
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        background: transparent;
        z-index: 1;
        opacity: 0.5;
        cursor: not-allowed;
      }
    }
  }
  .el-picker-panel__body {
    .el-picker-panel__content.el-date-range-picker__content {
        .el-date-table{
          td{
            &.in-range {
              div{
                background-color: #fdf3c9!important;
              }
            }
            &.end-date,&.start-date{
              &.today{
                span{
                  color:#fff!important;
                }
              }
              span{
                background-color: #f9c901;
              }
            }
          }
        }
    }
    .el-time-panel{
      width: 0.83rem*25;
      right: 0;
      left: auto;
      .el-time-panel__btn.confirm{
        color:#f9c901!important;
      }
    }
  }
  .el-picker-panel__footer{
    justify-content: center;
    display: flex;
    .is-plain{
      width: 120px;
    }
  }
  &.hour-only{
    .el-time-panel{
      max-width: 100%;
    }
    .el-time-spinner{
      .el-time-spinner__wrapper{
        &:nth-child(2){
          display:none;
        }
        &:nth-child(1){
          width: 100%;
        }
      }
    }
  }
}
.el-picker-panel{
  .el-date-table td.today span{
    color: #f9c901!important;
  }
  .el-date-table td.today.current span{
    color: #fff!important;
  }
  .el-button.el-picker-panel__link-btn {
    background-color: #f9c901!important;
    color:#000!important;
    border: 1px solid #f9c901!important;
  }
  td.current:not(.disabled) span{
    background-color:#f9c901!important;
  }
}
.day-range{
  width:100%;
  padding:0.83rem*0.5 0.83rem*1.5;
  .small{
    font-weight:500;
  }
  .search-range{
    font-size:0.83rem*1.2!important;
    padding: 0.83rem 0.83rem*1.5!important;
    margin: 0.83rem*0.25!important;
  }
  .line-yellow{
    &:first-child{
      margin-left:0!important;
    }
    &:last-child{
      margin-right:0!important;
    }
    font-size:0.83rem*1.2!important;
    padding:0.83rem!important;
    margin: 0.83rem*0.25!important;
    color: #f9c901;
    background: transparent;
    border: 0.83rem*0.1 solid #f9c901;
    border-radius:0.83rem*0.3;
    &.active{
      background:#f9c901!important;
      color:#333!important;
    }
  }
}
.date-time-picker-box{
  display: flex;
  align-items:center;
  flex-wrap: wrap;
  .picker{
    width: 0.83rem*32;
    display: flex;
    align-items:center;
    margin-right: 0.83rem;
    &.date{
      //width: 0.83rem*32;
    }
    &.datetimerange{
      .el-range-input{
        padding: 0!important;
      }
      .el-range-separator{
        height: auto!important;
        line-height: 1!important;
        width: 0.83rem*2.5;
      }
    }
  }
  .el-input__prefix,.el-input__suffix{
    display:none;
  }
  .el-date-editor{
    height:0.83rem*3.2;
    width: 100%;
    input{
      font-size: 0.83rem*1.2;
      padding-left: 0.83rem*1;
      padding-right: 0.83rem*1;
      height:100%;
      width: 100%;
    }
  }
  .label{
    color:#fff;
    margin-left:0.83rem*0.7;
    margin-right:0.83rem*0.7;
    font-size:0.83rem*1.4;
  }
}
.item{
  margin:1rem 1rem 1rem 0rem;
}
</style>
