<template>
    <div class="flex">
        <div class="flex flex-col border-y border-t-gray-200 " v-if="direction=='left'">
            <div 
            class="flex  flex-col  border-r-2   "
            :class="{
                'handler  cursor-pointer p-1 px-3 text-sm  text-center  ':true,
                ' border-r-blue-600  bg-gray-100 ':this.active==i,
            }"  v-for="(e,i) in data" :key="e.title" @click="changeSelect(i)">
                <i :class="e.icon" class="text-3xl" ></i>
                <p class=" text-xs">{{e.title}}</p>
            </div>
        </div>
        <KeepAlive>
            <component 
            style="z-index:9999"
            class="bg-white   content flex-grow overflow-auto border w-80 "
             :key="vnode.componemt" :is="vnode.componemt" ref="cur" @close="close" v-if="vnode!=null" />
        </KeepAlive>
        <div class="flex flex-col border-y border-t-gray-200 " v-if="direction=='right'">
            <div
            class="flex  flex-col   border-l-2"
             :class="{
                'handler   cursor-pointer  text-sm  text-center p-1  px-3 ':true,
                'border-l-blue-600  bg-gray-100':this.active==i,
            }" v-for="(e,i) in data" :key="e.title" @click="changeSelect(i)">
                <i :class="e.icon" class="text-3xl"></i>
                <p class=" text-xs">{{e.title}}</p>
            </div>
        </div>
    </div>
</template>

<style scoped>

</style>

<script lang="ts">
import { defineComponent } from 'vue'
import { KeepAlive } from 'vue'
import TabItem from './TabItem.vue'

export default defineComponent({
    props:{
        data:Array,
        direction:String,
        select:Number,
    },
    data() {
        return {
            active: 0,
        };
    },
    computed:{
      vnode(){
          if(this.active==-1){
              return null
          }else{
              return this.data[this.active];
          }
          
      } 
    },
    methods:{
        changeSelect(i:number){
            if(i==this.active){
                this.active=-1;
            }else{
                this.active=i;
            }
           
        },
        close(){
            this.active=-1;
        }
    },
    mounted() {
    },
    setup() {
    },
    components: { TabItem,KeepAlive ,}
})
</script>
