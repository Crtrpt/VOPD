<template>
  <component
    :class="{
      'border-green-500': isDragenter,
      'border-2': isDragenter,
      'border-dashed': isDragenter,
    }"
    @dragenter="dragenter($event)"
    @drop="drop($event)"
    @dragleave="dragleave($event)"
    @click="click($event)"
    @contextmenu="context($event)"
    class="node"
    v-bind:is="data.render"
    :data="data"
    v-if="!data.attr['isDelete'] && !data.attr['isEye']"
    @mousedown="mousedown($event, data)"
    @mouseenter="mouseenter($event,data)"
    @mouseleave="mouseleave($event,data)"
    ref="node"
  />
</template>

<script lang="ts">
import { defineComponent } from "vue";
import ev from "../utils/Eventbus";
import ContextMenu from "../common/context/ContextMenu.js"

export default defineComponent({
  props: {
    data: Object,
  },
  mounted() {  
    this.data.attr['el']=this.$el;
  },
  data() {
    return {
      isDragenter: false,
    };
  },
  methods: {
    context(e){
      e.stopPropagation();
      console.log("处理元素右击事件"+"===============")
      console.log(this.$el);
      const menu = new ContextMenu(this.$el, [
                  {text: '保存为组件', value: '保存为组件',  hotkey: 'F5', onclick: ()=>{}},
                  null,
                  {text: '层级', value: '保存为组件',hotkey: '❯',subitems:[
                    {
                      text:"向上一层",
                      hotkey: 'ctrl +'
                    },
                    {
                      text:"向下一层",
                      hotkey: 'ctrl -'
                    },
                    {
                      text:"到最上层",
                      hotkey: 'ctrl + alt +'
                    },
                    {
                      text:"到最下层",
                      hotkey: 'ctrl + alt -'
                    },
                  ] },
                  null,
                  {text: '删除组件', value: 'kali_dark', onclick: ()=>{}},
      ]);
      menu.install();
      menu.show();
    },
    drop(e) {
      var t=JSON.parse(e.dataTransfer.getData("text/plain"));
      this.$store.dispatch("page/addNode",{id:this.data.id,nodeId:t.id})     
      e.stopPropagation();
    },
    dragenter(e) {
      console.log("进入");
      this.isDragenter = true;
      e.stopPropagation();
    },
    dragleave(e) {
      console.log("离开");
      this.isDragenter = false;
      e.stopPropagation();
    },
    mousedown(e, n) {
      e.stopPropagation();
    },
    click(e){
          if(!this.data.attr['isLock']){
              this.$store.dispatch("page/setAttr",{id:this.data.id,attr:"isSelect",val:true})
              e.stopPropagation();
          }
    },
    mouseenter(e) {
       if(!this.data.attr['isLock']){
            this.$store.dispatch("page/setAttr",{id:this.data.id,attr:"isHover",val:true})
              e.stopPropagation();
      }
    },
    mouseleave(e) {
       if(!this.data.attr['isLock']){
            this.$store.dispatch("page/setAttr",{id:this.data.id,attr:"isHover",val:false})
              e.stopPropagation();
      }
    },
  },
});
</script>
