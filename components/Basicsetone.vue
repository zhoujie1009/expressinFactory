<template>
    <div class="tab_body tab_body1">
        <div class="item_content">
            <div class="basic_row" @click="basicfunc('origin')">
                <span class="left_txt">原图效果：</span>
                <img src="../css/demo/e5.png" class="img_wrapper" alt />
            </div>
            <div class="basic_row" @click="basicfunc('sketch')">
                <span class="left_txt">素描效果：</span>
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
	        let obj = canvas.getActiveObject();
	       	obj.filters[index] = filter;
		obj.applyFilters(canvas.renderAll.bind(canvas));
	    }
            basicfunc(type){
		let obj = canvas.getActiveObject();
	        if(type==="sketch"){
		   obj.grayscale = new f.grayscale();
		   obj.applyFilters(canvas.renderAll.bind(canvas));
		}
               //let imgTarget;
	      console.log(type+'------'); 
            }
        }
    }
</script>
