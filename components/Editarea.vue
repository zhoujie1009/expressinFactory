<template>
  <div class="edit_area">
  	<canvas id="canvas" width="1100" height="600"></canvas>
  </div>
</template>
<script type="text/javascript">
  window.onload = function() {
    window.canvas = new fabric.Canvas('canvas');
    var rect = new fabric.Rect({
        top : 50,
        left : 100,
        width : 1100,
        height : 600,
        fill : 'red'
    });
    canvas.add(rect);
    canvas.clear();
    canvas.remove(canvas.getActiveObject());
    canvas.add(new fabric.Rect({
      width: 50,
      height: 50,
      left: 50,
      top: 50,
      fill: 'rgb(255,0,0)'
    }));

    // add green, half-transparent circle
    canvas.add(new fabric.Circle({
      radius: 40,
      left: 50,
      top: 50,
      fill: 'rgb(0,255,0)',
      opacity: 0.5
    }));
  }
</script>
