<template>
  <div class="rk-board container">
    <div v-clickout="() => this.show = false">
      <div class="flex-c cp" @click="show = true">
        <span class="rk-board-app mr5">{{stateOptions.currentApplication.label}}</span>
        <ion-icon name="link" class="mr10" size="small"></ion-icon>
      </div>
      <RkToolDashboard :stateOptions="stateOptions" :show.sync="show"/>
    </div>
    <ion-icon name="arrow-forward" class="grey mr10"></ion-icon>
    <select v-if='stateOptions.currentService' :value="stateOptions.currentService.key" @change="serviceChange($event)" class="mr15" style="color:#fafafa;background: 0; border: 0; outline: none;">
      <option style="background-color: #2f333c;" v-for="i in stateOptions.services" :key="i.key" :value="i.key">{{i.label}}</option>
    </select>
    <span class="grey mr10">|</span>
    <select  v-if='stateOptions.currentServer' :value="stateOptions.currentServer.key" @change="serverChange($event)" class="grey mr15" style="background: 0; border: 0; outline: none;">
      <option style="background-color: #2f333c;" v-for="i in stateOptions.servers" :key="i.key" :value="i.key">{{i.pid+'@'+i.ipv4[0]}}</option>
    </select>
    <button class="rk-board-inspector" @click="$emit('showServer')">inspector</button>
  </div>
</template>

<script lang="ts">
import Vue from 'vue';
import { State } from '@/store/modules/options.ts';
import { Component, Prop } from 'vue-property-decorator';
import RkToolDashboard from '@/components/rk-tool-dashboard.vue';
import { serviceChange, serverChange } from '@/store/dispatch/dashboard.ts';

@Component({
  components: { RkToolDashboard },
})
export default class RkBoard extends Vue {
  @Prop() stateOptions: State;
  show:Boolean = false;
  serviceChange(e) {
    serviceChange(this.stateOptions.services[e.target.selectedIndex]);
  }
  serverChange(e) {
    serverChange(this.stateOptions.servers[e.target.selectedIndex]);
  }
}
</script>
<style lang="scss">
.rk-board{
  display: flex;
  flex-shrink: 0;
  align-items: center;
  padding-top: 20px;
  padding-bottom:  20px;
  color: #fafafa;
  // background-color: #f6f7fb;
  background-color: #2f333c;
}
.rk-board-app{
  font-size: 18px;
  font-weight: 600;
}
.rk-board-service{
  font-weight: 600;
}
.rk-board-inspector{
  cursor: pointer;
  border-radius: 4px;
  background-color: #4477dd;
  border: 0;
  color: #fff;
  outline: none;
  padding: 4px .7em;
  transition: background-color .3s;
  &:hover{
    background-color: #5286f0;
  }
}
</style>
