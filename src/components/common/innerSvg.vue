<template>
  <div v-bind:style='OutStyleObject' :id="outerSvgId">
    <!--这是图形-->
    <pcCircle   :pc-height='pcHeight' :pc-width='pcWidth' :inner-color='innerColor' v-if="type=='circle'"></pcCircle>
    <pcRect     :pc-height='pcHeight' :pc-width='pcWidth' :inner-color='innerColor' v-if="type=='rect'"></pcRect>
    <pcTriangle :pc-height='pcHeight' :pc-width='pcWidth' :inner-color='innerColor' v-if="type=='triangle'"></pcTriangle>
    <pcDiamond  :pc-height='pcHeight' :pc-width='pcWidth' :inner-color='innerColor' v-if="type=='diamond'"></pcDiamond>
    <pcTrapezium  :pc-height='pcHeight' :pc-width='pcWidth' :inner-color='innerColor' v-if="type=='trapezium'"></pcTrapezium>
    <pcParallelogram  :pc-height='pcHeight' :pc-width='pcWidth' :inner-color='innerColor' v-if="type=='parallelogram'"></pcParallelogram>
   <!--这是中间的字-->
    <inner-font :pc-height='pcHeight' :shw-inner-font="shwInnerFont" :cardId='cardId' :input-font="inputFont" :pc-width='pcWidth' :position='position'></inner-font>
    <!--这是下面的字-->
    <footerFont :font='footerFont' :shw-foot-font="shwFootFont" :font-size='fontSize' :pc-width='pcWidth' :position='position' style='background-color: rgba(250, 250, 250, 0);'></footerFont>
  </div>
</template>
<script>
  import footerFont from '@/components/common/footerFont'
  import innerFont from '@/components/common/innerFont'
  import pcCircle from '@/components/svg/circle'
  import pcRect from '@/components/svg/rect'
  import pcTriangle from '@/components/svg/triangle'
  import pcDiamond from '@/components/svg/diamond'
  import pcTrapezium from '@/components/svg/trapezium'
  import pcParallelogram from '@/components/svg/parallelogram'
  import $ from 'jquery'
  import util from '@/util.js'

  export default {
    name: 'inner-svg',
    props: {
      left: { // x坐标
        type: Number,
        default: 10
      },
      top: { // y坐标
        type: Number,
        default: 10
      },
      pcWidth: { // 宽度
        type: Number,
        default: 40
      },
      pcHeight: { // 高度
        type: Number,
        default: 40
      },
      innerColor: { // 颜色
        type: String,
        default: 'red'
      },
      position: { // 位置模式 relative为在展示栏 absolute为在拖动栏、点击出现图标
        type: String,
        default: 'relative'
      },
      shwFootFont: { // 显示脚部文字
        type: Boolean,
        default: false
      },
      shwInnerFont: { // 显示内部文字
        type: Boolean,
        default: true
      },
      footerFont: { // 底部文字
        type: String,
        default: '图形'
      },
      inputFont: { // 内文字
        type: String,
        default: ''
      },
      cardId: {  // card id
        type: String,
        default: '2asd123'
      },
      svgType: { // 图形的类型 1单击出现的图形 2左侧显示的图形 3右侧显示的图形
        type: Number,
        default: 3
      },
      options: { // 其他参数
        type: Object,
        default: function () {
          return {}
        }
      },
      type: {
        type: String,
        default: 'rect'
      }
    },
    components: {
      innerFont,
      pcCircle,
      pcRect,
      pcTriangle,
      pcDiamond,
      pcTrapezium,
      pcParallelogram,
      footerFont
    },
    data () {
      return {
        OutStyleObject: { // 外层样式
          border: '0px dotted black',
          width: this.pcWidth + 'px',
          height: this.pcHeight + 'px',
          'margin-top': '5px',
          'padding-top': '2px',
          'z-index': '100',
          opacity: '1'
        },
        InnerSvgStyleObject: { // 内部样式
          width: '0',
          height: '0',
          'border-bottom': this.pcWidth + 'px solid rgba(53, 109, 222, 0.3)',
          'border-left': this.pcHeight / 2 + 'px solid transparent',
          'border-right': this.pcHeight / 2 + 'px solid transparent'
        },
        fontSize: 10, // 所有字体大小
        outerSvgId: util.getUuid('outerSvg'),
        inputModel: { // 输入框属性
          inputValue: this.inputFont, // 输入的值
          inputRows: 1, // 行数
          showInput: false  // 是否显示输入框，否则显示文字
        }
      }
    },
    methods: {
      inputBlurFunction: function (event) { // 输入框离焦事件
        this.inputModel.showInput = false
        this.inputModel.inputRows = this.inputModel.inputValue.length / 6
      }
    },
    mounted: function () {
      var _this = this
      if (this.svgType === 2) {
        // 点击事件
        $('#' + this.outerSvgId).click(e => {
          _this.$emit('clickSvg') // 触发事件
        })
      }
    }
  }
</script>
