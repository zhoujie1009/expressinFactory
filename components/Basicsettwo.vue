<template>
    <div class="tab_body tab_body1">
        <div class="item_content">
            
            <div class="basic_row" @click="basicfunc('saturate')" style="height:130px;">
                <span class="left_txt">饱和度效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
                <br>
                <label>Value: <input type="range" value="100" min="-200" max="100" v-model="saturateVal"></label>
                <br>
            </div>
            
            <div class="basic_row" @click="basicfunc('contrast')" style="height:130px;">
                <span class="left_txt">对比度：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
                <br>
                <label>Value: <input type="range" value="0" min="-255" max="255" v-model="contrastVal"></label>
                <br>
            </div>
            <div class="basic_row" @click="basicfunc('removewhite')">
                <span class="left_txt">去重白效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
                <br>
                <label>Threshold: <input type="range" v-model="thresholdVal" value="60" min="0" max="255"></label>
                <br>
                <label>Distance: <input type="range" v-model="distanceVal" value="10" min="0" max="255"></label>
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
               
            },
            thresholdVal(val){
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
                
                if(this.filterType === 'removewhite'){
                    target.filters[0].threshold = val;
                }
                target.applyFilters(canvas.renderAll.bind(canvas));

            },
            distanceVal(val){
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
                if(this.filterType === 'removewhite'){
                    target.filters[0].distance = val;
                }
                 target.applyFilters(canvas.renderAll.bind(canvas));
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
                    if(type === 'saturate'){
                        //滤镜
                        filter = new this.f.Saturate({
                            saturate:this.saturateVal        
                        });
                    }else if(type === "contrast"){
                        //对比度
                       filter = new this.f.Contrast({
                            contrast:this.contrastVal
                        });
                    }else if(type === 'removewhite'){
                        filter = new fabric.Image.filters.RemoveWhite({
                            threshold:this.thresholdVal,
                            distance:this.distanceVal
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