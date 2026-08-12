<script setup>
import SampleOne from './components/practices/basic/SampleOne.vue'
import SampleTwo from './components/practices/basic/SampleTwo.vue';
import Vhtml from './components/practices/basic/V-html.vue';
import VText from './components/practices/basic/V-text.vue';
import Vbind from './components/practices/basic/V-bind.vue';
import XSS from './components/practices/basic/XSS.vue';
import VBind2 from './components/practices/basic/V-bind2.vue';
import StyleBinding from './components/practices/basic/StyleBinding.vue';
import Short from './components/practices/basic/BindShort.vue';
import Vif from './components/practices/basic/V-if.vue';
import Vshow from './components/practices/basic/V-show.vue';
import Vfor from './components/practices/basic/V-for.vue';
import VPre from './components/practices/basic/VPre.vue';
import VCloak from './components/practices/basic/VCloak.vue';
import VOnce from './components/practices/basic/VOnce.vue';
import VMemo from './components/practices/basic/VMemo.vue';
import VOn from './components/practices/basic/VOn.vue';
import EventObject from './components/practices/basic/EventObject.vue';
import EventModifier from './components/practices/basic/EventModifier.vue';
import VModel from './components/practices/basic/VModel.vue';
import FormData from './components/practices/basic/FormData.vue';
import VmodelModifiers from './components/practices/basic/VModelModifiers.vue';
import VueStyle from './components/practices/basic/VueStyle.vue';
import RefExample from './components/practices/basic/RefExample.vue';
import ReactiveExample from './components/practices/basic/ReactiveExample.vue';
import ComputedExample from './components/practices/basic/ComputedExample.vue';
import WatchExample from './components/practices/basic/WatchExample.vue';
import MultiSource from './components/practices/basic/MultiSource.vue';
import DeepWatch from './components/practices/basic/DeepWatch.vue';
import ReactiveWatch from './components/practices/basic/ReactiveWatch.vue';
import WatchEffect from './components/practices/basic/WatchEffect.vue';
import LifecycleHook from './components/practices/component/LifecycleHook.vue';
import ParentComponent from './components/practices/component/ParentComponent.vue';

</script>

<template>
  <div style="padding: 20px">
    <SampleOne />
    <SampleTwo />
    <Vhtml />
    <XSS />
    <VText />
    <Vbind />
    <VBind2 />
    <StyleBinding />
    <Short />
    <Vif />

    <Vshow />
    <Vfor />

    <VPre />
    <VCloak />
    <VOnce />
    <VMemo />
    <VOn />
    <EventObject />
    <EventModifier />
    <VModel />
    <FormData />
    <VmodelModifiers />
    <VueStyle />
    <RefExample />
    <ReactiveExample />

    <ComputedExample/>
    <WatchExample/>
    <MultiSource/>
    <DeepWatch/>
    <ReactiveExample/>
    <ReactiveWatch/>
    <WatchEffect/>

    <LifecycleHook/>

    <ParentComponent/>



  </div>
</template>
