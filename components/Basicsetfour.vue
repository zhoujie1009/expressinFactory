<template>
    <div class="tab_body tab_body1">
        <div class="item_content">
            <div class="body_row">
                <span class="left_txt" @click="addText()">添加文字</span>
            </div>
            <div class="body_row">
                <label for="drawing-mode-selector">文字大小:</label>
                <select style="color:black" v-model="selectMode">
                    <option v-for="mode in modes" >
                        {{mode}}    
                    </option>                 
                </select>
            </div>
            <div class="body_row">
                <label for="drawing-color">font color:</label>
                <input type="color" value="#005E7A" v-model="fontColor"><br>
            </div>
        </div>
    </div>
</template>
<script>
    export default{
        data(){
            return {
                modes:["24px","22px","20px","18px","16px","14px","12px","10px"],
                fontColor:'#333',
                selectMode:'24px',
            } 
        },
        watch:{
            selectMode(val){
                let target = canvas.getActiveObject();
                let fontSize = parseInt(val,10);
                if(target){
                    let styles = {};
                    styles.fontSize = fontSize;
                    target.setSelectionStyles(styles);
                    target.fontSize = fontSize;
                    target.applyFilters(canvas.renderAll.bind(canvas));
                }
            },
            fontColor(val){
                let target = canvas.getActiveObject();
                if(target){
                    let styles = {};
                    styles.fill = val;
                    target.setSelectionStyles(styles);
                    target.fill = val;
                    target.applyFilters(canvas.renderAll.bind(canvas));
                }
            }
        },
        methods:{
            addText(){
                let text = new fabric.IText('此处编辑文字',{
                    left: 10,
                    top: 20,
                    fontFamily: 'Courier',
                    fill: '#000000',
                    type:'font',
                    fontSize:16,
                    textAlign:'center',
                    isEditing:true,
                    styles:{
                    }
                });
                canvas.add(text);
            }
        }
    }
</script>