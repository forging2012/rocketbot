<template>
<rk-panel class="rk-application-throughput" title="Application Throughput">
  <div class="mb15" v-for="i in fiveData" :key="i.key">
    <div>
      <span class="r sm">{{i.value}} calls/ m</span>
      <div class="ell mb5 cp link-hover" style="max-width: 160px;" @click="appChange(i)"><span v-tooltip="{
        content: i.label,
        trigger: 'hover',
      }">{{i.label}}</span></div>
    </div>
    <RkProgress :precent="i.value/maxValue*100"/>
  </div>
</rk-panel>
</template>

<script lang="ts">
import Vue from 'vue';
import { State } from 'vuex-class';
import { Component } from 'vue-property-decorator';
import { appChange } from '@/store/dispatch/dashboard.ts';

@Component({})
export default class RkChartBox extends Vue {
  @State('dashboard') stateDashboard;
  changeApp = appChange;
  appChange(i) {
    const temp = { key: `${i.key}`, label: i.label };
    this.changeApp(temp);
  }
  get fiveData() {
    return [...this.stateDashboard.applicationThroughput].splice(0, 5);
  }
  get maxValue() {
    const temp:Number[] = this.fiveData.map(i => i.value);
    return Math.max.apply(null, temp);
  }
}
</script>
<style lang="scss">
.rk-application-throughput{
  .rk-progress-inner{
    background-color:#bf99f8 !important;
  }
}
</style>
