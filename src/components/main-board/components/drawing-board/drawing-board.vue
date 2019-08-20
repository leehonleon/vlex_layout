<template>
  <div class="content-split">
    <Split v-model="split2" mode="vertical">
      <div slot="top" class="PreviewFrame">
        <div class="UrlBar" id="UrlBar">
          <div class="UrlBar-controls">
            <Button type="text" class="UrlBar-controlBtn" disabled>
              <Icon type="md-arrow-back" /></Button>
            <Button type="text" class="UrlBar-controlBtn" disabled>
              <Icon type="md-arrow-forward" /></Button>
            <Button type="text" class="UrlBar-controlBtn">
              <Icon type="md-refresh" /></Button>
          </div>
          <div class="UrlBar-searchWrapper">
            <AutoComplete v-model="value1" :data="data1" @on-search="handleSearch1" placeholder="input here" style="width:400px"></AutoComplete>
          </div>
        </div>
      </div>
      <div slot="bottom" class="Console">
        Console Pane
      </div>
    </Split>
  </div>
</template>

<script>
import './drawing-board.less'
export default {
  name: 'Drawingboard',
  components: {
  },
  data () {
    return {
      split2: 0.93,
      value1: '',
      data1: []
    }
  },
  mounted () {
    //
  },
  methods: {
    handleSearch1 (value) {
      this.data1 = !value ? [] : [
        value,
        value + value,
        value + value + value
      ]
    }
  }
}
</script>
<style>
.demo-split {
  height: 100%;
  border: 1px solid #dcdee2;
}
.demo-split-pane {
  padding: 10px;
}

.UrlBar {
  z-index: 1;
  background: #f0f0f0;
  padding: 2px 0;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  border-bottom: 1px solid #d1d1d1;
  position: relative;
  box-shadow: -1px 1px 2px rgba(0, 0, 0, 0.1);
}
#PreviewContentWrapper {
  position: relative;
}
.UrlBar-controls {
  -webkit-box-pack: justify;
  -ms-flex-pack: justify;
  justify-content: space-between;
  padding: 0 6px;
  width: 120px;
}
.UrlBar-controls,
.UrlBar-searchWrapper {
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  -webkit-box-align: center;
  -ms-flex-align: center;
  align-items: center;
}
.UrlBar-controls .UrlBar-controlBtn[disabled],
.UrlBar-controlBtn[disabled]:hover,
.UrlBar-controlBtn[disabled].active {
  background: transparent;
}
.UrlBar .UrlBar-controlBtn {
  cursor: default !important;
  background: transparent;
  transition: all 0.2s ease-in-out;
  padding: 0 5px;
  display: -webkit-box;
  display: -ms-flexbox;
  display: flex;
  color: #626262;
  font-size: 21px;
  border-radius: 2px;
  border: 0px;
}

.UrlBar .ivu-btn-text.disabled:hover,
.UrlBar .ivu-btn-text[disabled]:hover,
.UrlBar .ivu-btn-text[disabled]:active {
  background: transparent;
  background-color: transparent;
}
.UrlBar .ivu-btn-text:hover,
.UrlBar .ivu-btn-text:active {
  background: transparent;
  background-color: transparent;
}
</style>
