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
                <span class="left_txt">像素化效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
            </div>
	     <div class="basic_row" @click="basicfunc('removewhite')">
                <span class="left_txt">去重白效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
            </div>
	    
        </div>
    </div>
</template>
<script>
    export default{
        data(){
            return {
		f:fabric.Image.filters
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
	        if(type==="grayscale"){	
                   filter = new fabric.Image.filters.Grayscale();
		}else if(type === 'removewhite'){
		   filter = new fabric.Image.filters.RemoveWhite();
		}else if(type === 'pixelate'){
		   filter = new fabric.Image.filters.Pixelate(); 	
		}
		target.filters.push(filter);
		target.applyFilters(canvas.renderAll.bind(canvas));
            }
        }
    }
</script>
