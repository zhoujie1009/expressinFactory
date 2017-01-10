<template>
    <div class="tab_body tab_body1">
        <div class="item_content">
            <div class="basic_row" @click="basicfunc('invert')">
                <span class="left_txt">反相效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
            </div>
            <div class="basic_row" @click="basicfunc('saturate')" style="height:130px;">
                <span class="left_txt">饱和度效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
                <br>
                <label>Value: <input type="range" value="100" min="-200" max="100" v-model="saturateVal"></label>
                <br>
            </div>
            <div class="basic_row" @click="basicfunc('emboss')">
                <span class="left_txt">浮雕效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
            </div>
            <div class="basic_row" @click="basicfunc('sharpen')">
                <span class="left_txt">锐化效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
            </div>
            <div class="basic_row" @click="basicfunc('contrast')">
                <span class="left_txt">对比度：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
                <br>
                <label>Value: <input type="range" value="0" min="-255" max="255" v-model="contrastVal"></label>
                <br>
            </div>
        </div>
    </div>
</template>
<script>
    export default{
        data(){
            return {
		        f:fabric.Image.filters,
                contrastVal:0,
                saturateVal:100,
                filterType:'',
            }
        },
        watch:{
            contrastVal(val){
                let objects = canvas._objects;
                let target;
                target = canvas.getActiveObject();
                if(!target){
                    for(let i=0;i<objects.length;i++){
                        if(objects[i].type === 'image'){
                            target = objects[i];
                        }
                    }
                }
                if(!target){
                    return;
                }
                if(this.filterType === 'contrast'){
                    target.filters[0].contrast = val;
                }
            },
            saturateVal(val){
                let objects = canvas._objects;
                let target;
                target = canvas.getActiveObject();
                if(!target){
                    for(let i=0;i<objects.length;i++){
                        if(objects[i].type === 'image'){
                            target = objects[i];
                        }
                    }
                }
                if(!target){
                    return;
                }
                if(this.filterType === 'saturate'){
                    target.filters[0].saturate = val;
                }
               
            }
        },
        methods:{
            applyFilter(indx,filter){
                let obj = canvas.getActiveObject() ?canvas.getActiveObject():canvas.getActiveGroup();
                obj.filters[index] = filter;
                obj.applyFilters(canvas.renderAll.bind(canvas));
	        },
            basicfunc(type){
                let obj = canvas;
                let objects = canvas._objects;
                let target,filter;
                target = canvas.getActiveObject();
                if(!target){
                    for(let i=0;i<objects.length;i++){
                        if(objects[i].type === 'image'){
                            target = objects[i];
                        }
                    }
                }
                if(!target){
                    return;
                }
                this.filterType = type;
                if(type === 'origin'){
                    target.filters = [];
                }else{
                    if(type==="invert"){	
                        //反相
                        filter = new this.f.Invert();
                    }else if(type === 'saturate'){
                        //滤镜
                        filter = new this.f.Saturate({
                            saturate:this.saturateVal        
                        });
                    }else if(type === 'sharpen'){
                        //锐化
                        filter = new this.f.Convolute({
                            matrix:[
                                0,-1,0,
                                -1,5,-1,
                                0,-1,0
                            ]        
                        });
                    }else if(type === 'emboss'){
                        //浮雕
                        filter = new this.f.Convolute({
                            matrix:[
                                1,1,1,
                                1,0.7,-1,
                                -1,-1,-1
                            ]
                        });    
                    }else if(type === "contrast"){
                        //对比度
                       filter = new this.f.Contrast({
                            contrast:this.contrastVal
                        });
                    }
                    target.filters = [];
                    target.filters.push(filter);
                }
                target.applyFilters(canvas.renderAll.bind(canvas));
            }
        }
    }
</script>