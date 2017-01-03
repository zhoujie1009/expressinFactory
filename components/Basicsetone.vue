<template>
    <div class="tab_body tab_body1">
        <div class="item_content">
            <div class="basic_row">
                <span class="left_txt">原图：</span>
                <img src="../css/demo/e5.png" class="imgWrapper" alt />
            </div>
            <div class="basic_row">
                <span class="left_txt">素描效果：</span>
                <img src="../css/demo/e5.png" class="imgWrapper" alt />
            </div>
            <div class="basic_row">
                <span class="left_txt">黑白效果：</span>
                <img src="../css/demo/e5.png" class="imgWrapper" alt />
            </div>
            <div class="basic_row">
                <span class="left_txt">灰白效果：</span>
                <img src="../css/demo/e5.png" class="imgWrapper" alt />
            </div>
        </div>
        <div class="item_content">
            <div class="body_row" @click="basicfunc()">
                <span class="left_txt">亮度：</span>
                <img src="../css/demo/e5.png" class="imgWrapper" alt />
            </div>
            <div class="body_row">
                <span class="left_txt">对比度：</span>
                <span class="right_txt">效果图片</span>
            </div>
            <div class="body_row">
                <span class="left_txt">饱和度：</span>
                <span class="right_txt">效果图片</span>
            </div>
            <div class="body_row">
                <span class="left_txt">色相：</span>
                <span class="right_txt">效果图片</span>
            </div>
        </div>
    </div>
</template>
<script>
    export default{
        data(){
            return {

            }
        },
        methods:{
            basicfunc(){

            }
        }
    }
</script>