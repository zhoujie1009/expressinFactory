<template>
    <div class="tab_body tab_body1">
        <div class="item_content">
            <div class="basic_row" @click="basicfunc('origin')">
                <span class="left_txt">原图效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
            </div>
            <div class="basic_row" @click="basicfunc('grayscale')">
                <span class="left_txt">灰度效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
            </div>
	        <div class="basic_row" @click="basicfunc('pixelate')">
                <span class="left_txt">马赛克效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
            </div>
	     <div class="basic_row" @click="basicfunc('removewhite')">
                <span class="left_txt">去重白效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
            </div>
            <br>
            <label>Threshold: <input type="range" v-model="thresholdVal" value="60" min="0" max="255"></label>
            <br>
	        <label>Distance: <input type="range" v-model="distanceVal" value="10" min="0" max="255"></label>
        </div>
    </div>
</template>
<script>
    export default{
        data(){
            return {
		        f:fabric.Image.filters,
                thresholdVal:60,
                distanceVal:10,
                filterType:""
            }
        },
        watch:{
            thresholdVal(val){
                let objects = canvas._objects;
                let target;
                for(let i=0;i<objects.length;i++){
                    if(objects[i].type === 'image'){
                        target = objects[i];
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
                for(let i=0;i<objects.length;i++){
                    if(objects[i].type === 'image'){
                        target = objects[i];
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
                for(let i=0;i<objects.length;i++){
                    if(objects[i].type === 'image'){
                        target = objects[i];
                    }
                }
                if(!target){
                    return;
                }
                this.filterType = type;
                if(type === 'origin'){
                    target.filters = [];
                }else{
                    if(type==="grayscale"){	
                        filter = new fabric.Image.filters.Grayscale();
                    }else if(type === 'removewhite'){
                        filter = new fabric.Image.filters.RemoveWhite({
                            threshold:this.thresholdVal,
                            distance:this.distanceVal
                        });
                    }else if(type === 'pixelate'){
                        filter = new fabric.Image.filters.Pixelate(); 	
                    }
                    target.filters = [];
                    target.filters.push(filter);
                }
                target.applyFilters(canvas.renderAll.bind(canvas));
            }
        }
    }
</script>
