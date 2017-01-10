<template>
  <div class="edit_area">
  	<canvas id="canvas" width="800" height="500"></canvas>
  </div>
</template>
<script type="text/javascript">
  window.onload = function() {
    window.canvas = new fabric.Canvas('canvas',{
      isDrawingMode:false
    });
    var copyCanvas = document.getElementById('copyCanvas');
    copyCanvas.addEventListener('click', function(e){
         var target = canvas.getActiveObject();
         debugger
         if(target){
            target.index = window.globaIndex;
            window.globaIndex += 1;
         }else{
            return;
         }
    }, false);
  }
</script>
