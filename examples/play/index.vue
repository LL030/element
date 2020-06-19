<template>
  <div style="margin: 20px;">
    <el-input v-model="input" placeholder="请输入内容"></el-input>
    <div class="w-100 h-100">
      <div class="comp selected-filter custom d-inline-block" style="width: auto;" v-if="options && options.length>0">
        <el-select 
          v-if="multi || autocomplete" 
          ref="multiSelect"
          :multiple="multi"
          :filterable="autocomplete"
          collapse-tags
          :reserve-keyword="true"
          popper-class="custom-dropdown" 
          v-model="valueArr" :placeholder="label"
          @visible-change="filterQuery=''"
        >
          <el-option
            v-for="item in options"
            :key="item.value"
            :label="item.label"
            :value="item.value">
          </el-option>
        </el-select>
     </div>
    </div>
  </div>
</template>

<script>
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
        options:[
          {
            "label":"AEB001","value":"1029","parent":"101"},{"label":"AEB002","value":"1030","parent":"101"},{"label":"AEB003","value":"1031","parent":"101"},{"label":"AEB004","value":"1032","parent":"101"},{"label":"AEB005","value":"1033","parent":"101"},{"label":"AEB006","value":"1034","parent":"101"},{"label":"B001","value":"1","parent":"101"},{"label":"B002","value":"2","parent":"101"},{"label":"B003","value":"3","parent":"101"},{"label":"B004","value":"4","parent":"101"},{"label":"B005","value":"5","parent":"101"},{"label":"B006","value":"6","parent":"101"},{"label":"B007","value":"7","parent":"101"},{"label":"B008","value":"8","parent":"101"},{"label":"B009","value":"9","parent":"101"},{"label":"B010","value":"10","parent":"101"},{"label":"B011","value":"11","parent":"101"},{"label":"B012","value":"12","parent":"101"},{"label":"B013","value":"13","parent":"101"},{"label":"B014","value":"14","parent":"101"},{"label":"B015","value":"15","parent":"101"},{"label":"B016","value":"1017","parent":"101"},{"label":"B017","value":"1016","parent":"101"},{"label":"B018","value":"1020","parent":"101"},{"label":"B019","value":"1021","parent":"101"},{"label":"B091","value":"40","parent":"101"},{"label":"B092","value":"41","parent":"101"},{"label":"B093","value":"42","parent":"101"},{"label":"B094","value":"43","parent":"101"},{"label":"B095","value":"44","parent":"101"},{"label":"C001","value":"22","parent":"104"},{"label":"C002","value":"23","parent":"104"},{"label":"C003","value":"24","parent":"104"},{"label":"C004","value":"25","parent":"104"},{"label":"C005","value":"26","parent":"104"},{"label":"C006","value":"27","parent":"104"},{"label":"C091","value":"50","parent":"104"},{"label":"C092","value":"51","parent":"104"},{"label":"C093","value":"52","parent":"104"},{"label":"C094","value":"53","parent":"104"},{"label":"C095","value":"54","parent":"104"},{"label":"D001","value":"36","parent":"301"},{"label":"D002","value":"37","parent":"301"},{"label":"F005","value":"1006","parent":"901"},{"label":"F006","value":"1007","parent":"901"},{"label":"F007","value":"1008","parent":"901"},{"label":"F010","value":"1009","parent":"901"},{"label":"F011","value":"1010","parent":"901"},{"label":"F012","value":"1011","parent":"901"},{"label":"G001","value":"1003","parent":"901"},{"label":"G002","value":"1004","parent":"901"},{"label":"G003","value":"1005","parent":"901"},{"label":"J001","value":"99","parent":"801"},{"label":"NN1","value":"80","parent":"801"},{"label":"NN2","value":"81","parent":"801"},{"label":"NN3","value":"82","parent":"801"},{"label":"Q001","value":"28","parent":"103"},{"label":"Q002","value":"29","parent":"103"},{"label":"Q003","value":"30","parent":"103"},{"label":"Q004","value":"31","parent":"103"},{"label":"Q005","value":"32","parent":"103"},{"label":"Q006","value":"33","parent":"103"},{"label":"Q007","value":"1018","parent":"103"},{"label":"Q008","value":"1019","parent":"101"},{"label":"QA080","value":"83","parent":"801"},{"label":"QA081","value":"86","parent":"801"},{"label":"QA082","value":"85","parent":"801"},{"label":"QA083","value":"84","parent":"801"},{"label":"QA084","value":"100","parent":"801"},{"label":"QA090","value":"96","parent":"801"},{"label":"QA091","value":"95","parent":"801"},{"label":"QA092","value":"94","parent":"801"},{"label":"QA093","value":"93","parent":"801"},{"label":"QA094","value":"92","parent":"801"},{"label":"QA095","value":"91","parent":"801"},{"label":"QA096","value":"90","parent":"801"},{"label":"QA097","value":"89","parent":"801"},{"label":"QA098","value":"88","parent":"801"},{"label":"QA099","value":"87","parent":"801"},{"label":"QA100","value":"101","parent":"801"},{"label":"QA101","value":"102","parent":"801"},{"label":"QA102","value":"103","parent":"801"},{"label":"QA103","value":"104","parent":"801"},{"label":"QA104","value":"105","parent":"801"},{"label":"QA106","value":"106","parent":"801"},{"label":"QA107","value":"107","parent":"801"},{"label":"QA108","value":"108","parent":"801"},{"label":"QA109","value":"109","parent":"801"},{"label":"QA110","value":"110","parent":"801"},{"label":"QA111","value":"111","parent":"801"},{"label":"QA112","value":"112","parent":"801"},{"label":"QA113","value":"113","parent":"801"},{"label":"QA114","value":"114","parent":"801"},{"label":"QA115","value":"115","parent":"801"},{"label":"QA116","value":"116","parent":"801"},{"label":"QA117","value":"117","parent":"801"},{"label":"QA118","value":"118","parent":"801"},{"label":"QA119","value":"119","parent":"801"},{"label":"QA120","value":"120","parent":"801"},{"label":"QA121","value":"121","parent":"801"},{"label":"QA122","value":"122","parent":"801"},{"label":"QA123","value":"123","parent":"801"},{"label":"QA124","value":"124","parent":"801"},{"label":"QA125","value":"125","parent":"801"},{"label":"QA126","value":"126","parent":"801"},{"label":"qA127","value":"127","parent":"801"},{"label":"QA128","value":"128","parent":"801"},{"label":"QA129","value":"129","parent":"801"},{"label":"QA130","value":"130","parent":"801"},{"label":"QA131","value":"131","parent":"801"},{"label":"QA132","value":"132","parent":"801"},{"label":"QA133","value":"133","parent":"801"},{"label":"QA134","value":"134","parent":"801"},{"label":"QA135","value":"135","parent":"801"},{"label":"QA136","value":"136","parent":"801"},{"label":"QA137","value":"137","parent":"801"},{"label":"QA138","value":"138","parent":"801"},{"label":"QA139","value":"139","parent":"801"},{"label":"QA140","value":"140","parent":"801"},{"label":"QA141","value":"141","parent":"801"},{"label":"QA142","value":"142","parent":"801"},{"label":"QA143","value":"143","parent":"801"},{"label":"QA144","value":"144","parent":"801"},{"label":"QA145","value":"145","parent":"801"},{"label":"QA146","value":"146","parent":"801"},{"label":"QA147","value":"147","parent":"801"},{"label":"QA148","value":"148","parent":"801"},{"label":"QA149","value":"149","parent":"801"},{"label":"QA150","value":"150","parent":"801"},{"label":"qa_testing2162018","value":"97","parent":"801"},{"label":"R001","value":"38","parent":"401"},{"label":"R002","value":"39","parent":"401"},{"label":"RedBull","value":"1015","parent":"801"},{"label":"ROBOT_001","value":"1001","parent":"901"},{"label":"ROBOT_002","value":"1002","parent":"901"},{"label":"S001","value":"34","parent":"201"},{"label":"S002","value":"35","parent":"201"},{"label":"testING","value":"98","parent":"801"},{"label":"V001","value":"16","parent":"102"},{"label":"V002","value":"17","parent":"102"},{"label":"V003","value":"18","parent":"102"},{"label":"V004","value":"19","parent":"102"},{"label":"V005","value":"20","parent":"102"},{"label":"V006","value":"21","parent":"102"},{"label":"V007","value":"55","parent":"102"},{"label":"V008","value":"56","parent":"102"},{"label":"V009","value":"57","parent":"102"},{"label":"V010","value":"58","parent":"102"},{"label":"V091","value":"45","parent":"102"},{"label":"V092","value":"46","parent":"102"},{"label":"V093","value":"47","parent":"102"},{"label":"V094","value":"48","parent":"102"},{"label":"V095","value":"49","parent":"102"},{"label":"WTF001","value":"1012","parent":"901"
          }
        ]
      };
    },
    methods: {
      selectAll(isSelectAll){
        if(isSelectAll){
          let op = []
          this.options.map(x=>{
            op.push(x.value)
          })
          this.valueArr = op
        }else{
          this.valueArr = []
        }
      },
    }
  };
</script>
