<template>
    <div class="tab_body tab_body1">
        <div class="item_content">
            <div class="body_row">
                <div id="drawing-mode-options">
                    <label>开启涂鸦</label>
                    <input type="checkbox" id="checkbox" v-model="modelChecked">
                    <br>
                    <label for="drawing-mode-selector">Mode:</label>
                    <select style="color:black" v-model="selectMode">
                        <option v-for="mode in modes" v-bind:value="{{mode}}">
                            {{mode}}    
                        </option>                 
                    </select>
                    <br>
                    <label for="drawing-line-width">Line width:</label>
                    <span class="info">{{lineWidth}}</span><input type="range" value="30" min="0" max="50" v-model="lineWidth"><br>

                    <label for="drawing-color">Line color:</label>
                    <input type="color" value="#005E7A" v-model="lineColor"><br>

                    <label for="drawing-shadow-color">Shadow color:</label>
                    <input type="color" value="#005E7A" v-model="shadowColor"><br>

                    <label for="drawing-shadow-width">Shadow width:</label>
                    <span class="info">{{shadowWidth}}</span><input type="range" value="0" min="0" max="50" v-model="shadowWidth"><br>

                    <label for="drawing-shadow-offset">Shadow offset:</label>
                    <span class="info">{{shadowOffset}}</span><input type="range" value="0" min="0" max="50" v-model="shadowOffset"><br>
                </div>
            </div>
            
        </div>
    </div>
</template>
<script>
    export default{
        data(){
            return {
                modes:["Pencil","Circle","Spray","Pattern","hline","vline","square","diamond"],
                brushPatterns:{},
                selectMode:'Pencil',
                lineWidth:15,
                shadowColor:"#005E7A",
                lineColor:"#005E7A",
                shadowWidth:0,
                shadowOffset:0,
                modelChecked:false
            }
        },
        ready(){
            
            canvas.isDrawingMode = this.modelChecked;
            if(fabric.PatternBrush){
               let vlinePatternBrush = new fabric.PatternBrush(canvas);
               vlinePatternBrush.getPatternSrc = function(){
                   let patternCanvas = fabric.document.createElement('canvas');
                   patternCanvas.width = patternCanvas.height = 10;
                   let ctx = patternCanvas.getContext('2d');
                   ctx.strokeStyle = this.lineColor;
                   ctx.lineWidth = 5;
                   ctx.beginPath();
                   ctx.moveTo(0,5);
                   ctx.lineTo(10,5);
                   ctx.closePath();
                   ctx.stroke();

                   return patternCanvas;
               } 
               this.brushPatterns.vline = vlinePatternBrush;

               let hLinePatternBrush = new fabric.PatternBrush(canvas);
               hLinePatternBrush.getPatternSrc = function(){
                   let patternCanvas = fabric.document.createElement('canvas');
                   patternCanvas.width = patternCanvas.height = 10;
                   let ctx = patternCanvas.getContext('2d');
                   ctx.strokeStyle = this.lineColor;
                   ctx.lineWidth = 5;
                   ctx.beginPath();
                   ctx.moveTo(5,0);
                   ctx.lineTo(5,10);
                   ctx.closePath();
                   ctx.stroke();
                   return patternCanvas;
               }
               this.brushPatterns.hline = hLinePatternBrush;

               let squarePatternBrush = new fabric.PatternBrush(canvas);
               squarePatternBrush.getPatternSrc = function(){
                   let squareWidth = 10,suqareDistance = 2;
                   let patternCanvas = fabric.document.createElement('canvas');
                   patternCanvas.width = patternCanvas.height = squareWidth+squareDistance;
                   let ctx = patternCanvas.getContext('2d');
                   ctx.fillStyle = this.lineColor;
                   ctx.fillRect(0,0,squareWidth,quareWidth);
                   return patternCanvas;
               }

               this.brushPatterns.square = squarePatternBrush;

               let diamondPatternBrush = new fabric.PatternBrush(canvas);
               diamondPatternBrush.getPatternSrc = ()=>{
                   let squareWidth = 10,suqareDistance = 5;
                   let patternCanvas = fabric.document.createElement('canvas');
                   let rect = new fabric.Rect({
                       width:squareWidth,
                       height:suqareWidth,
                       angle:45,
                       fill:this.lineColors
                   });
                   
                   var canvasWidth = rect.getBoundingRectWidth();
                   patternCanvas.width = patternCanvas.height = canvasWidth + squareDistance;
                   rect.set({left:canasWidth / 2, top: canvasWidth / 2});

                   var ctx = patternCanvas.getContext('2d');
                   rect.render(ctx);
                   return patternCanvas; 
               };          
               this.brushPatterns.diamond = diamondPatternBrush;
            }
            if(canvas.freeDrawingBrush){
                canvas.freeDrawingBrush.color = this.lineColor;
                canvas.freeDrawingBrush.width = parseInt(this.lineWidth,10) || 1;
                canvas.freeDrawingBrush.shadowBlur = 0;
            }

        },
        watch:{
            selectMode(value){
                console.log("-------"+value);
                if(this.brushPatterns[value]){
                    canvas.freeDrawingBrush = this.brushPatterns[value];
                }else{
                    canvas.freeDrawingBrush = new fabric[value + 'Brush'](canvas);
                }
                if(canvas.freeDrawingBrush){
                    canvas.freeDrawingBrush.color = this.lineColor;
                    canvas.freeDrawingBrush.width = parseInt(this.lineWidth,10) || 1;
                    canvas.freeDrawingBrush.shadowBlur = parseInt(this.shadowWidth,10) || 0;
                }
            },
            lineWidth(value){
                canvas.freeDrawingBrush.width = parseInt(this.lineWidth,10);
            },
            lineColor(value){
                canvas.freeDrawingBrush.color = this.lineColor;
            },
            shadowColor(value){
                canvas.freeDrawingBrush.shadowColor = this.shadowColor;
            },
            shadowWidth(value){
                canvas.freeDrawingBrush.shadowWidth = this.shadowWidth;
            },
            shadowOffset(value){
                canvas.freeDrawingBrush.shadowOffsetX = canvas.freeDrawingBrush.shadowOffsetY = parseInt(value,10) || 0;
            },
            modelChecked(){
                canvas.isDrawingMode = this.modelChecked;
            }    
        },
        methods:{
        }
    }
</script>