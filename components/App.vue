<template>
    <div class="wrapper">
        <div class="nav"><span class="red_font">Expression</span>Factory</div>
        <div class="content_wrap">
            <Sidertool></Sidertool>
            <div class="main_content">
                <Headtool></Headtool>
                <Editarea></Editarea>
                <Foottool></Foottool>
            </div>
        </div>
    </div>
</template>


<script>
import Sidertool from './Sidertool.vue'
import Headtool from './Headtool.vue'
import Editarea from './Editarea.vue'
import Foottool from './Foottool.vue'

export default {    
  components: {
    Sidertool,
    Headtool,
    Editarea,
    Foottool
  }
}
</script>
