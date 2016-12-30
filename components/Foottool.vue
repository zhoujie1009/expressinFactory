<template>
    <div class="magic_area">
        <div class="setting_tab">
            <span 
                class="tab_item"
                v-for="tab in tabs"
                :class="{'tab_active': $index === selected}"
                @click="choose($index)">{{tab.tabName}}</span>
        </div>
        <component :is="currentView" transition="fade" transition-mode="out-in"></component>

    </div>
</template>

<script> 
import Basicsetone from './Basicsetone.vue'
import Basicsettwo from './Basicsettwo.vue'
import Basicsetthree from './Basicsetthree.vue'
import Basicsetfour from './Basicsetfour.vue'

export default{
    data(){
        return{
            tabs:[
                {tabName:"基本设置1"},
                {tabName:"基本设置2"},
                {tabName:"基本设置3"},
                {tabName:"基本设置4"}
            ],
            selected:0,
            currentView:'set_tab_0'
        }
    },
    methods:{
        choose(index){
            this.selected = index;
            this.currentView = 'set_tab_' + index;
        }
    },
    components:{
        'set_tab_0':Basicsetone,
        'set_tab_1':Basicsettwo,
        'set_tab_2':Basicsetthree,
        'set_tab_3':Basicsetfour
    }
}
</script>