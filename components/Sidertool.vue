<template>
	<div class="sider_bar">
      <span class="bar_item bar_active importImg">
        <input type="file" id="addImg" class="addImg">
        <img id="ImgPr" width="300" height="300" class="dn"/>
      </span>
	    <span class="bar_item" onClick="rasterize()"><i class="fa fa-sign-out mr18"></i>导出</span>
	    <span class="bar_item"><i class="fa fa-floppy-o mr18"></i></i>保存</span>
	</div>
</template>
<script>
  var canvas;
  var getRandomInt;
  window.onload = function() {
    canvas = new fabric.Canvas('canvas');
    getRandomInt = fabric.util.getRandomInt;
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

    // add red rectangle
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
  function addImage(imgAdd, minScale, maxScale) {
        var coord = getRandomLeftTop();

        fabric.Image.fromURL(imgAdd, function(image) {
          image.set({
            left: coord.left,
            top: coord.top,
            angle: getRandomInt(-10, 10)
          })
          .scale(getRandomNum(minScale, maxScale))
          .setCoords();
          canvas.add(image);
        });
  }
  function setImg(){
        var imgUrl = $('#ImgPr').attr('src');
        var randomVal1 = Math.random();
        var randomVal2 = Math.random();

        addImage(imgUrl,Math.min(randomVal1, randomVal2),Math.max(randomVal1, randomVal2));
  }
  function getRandomNum(min, max) {
    return Math.random() * (max - min) + min;
  }
  function getRandomLeftTop() {
    var offset = 50;
    return {
        left: fabric.util.getRandomInt(0 + offset, 700 - offset),
        top: fabric.util.getRandomInt(0 + offset, 500 - offset)
    };
  }
  window.rasterize = function() {
    if (!fabric.Canvas.supports('toDataURL')) {
        alert('This browser doesn\'t provide means to serialize canvas to an image');
    }else {
        window.open(canvas.toDataURL('png'));
    }
  };
  window.rasterizeSVG = function() {
    window.open(
      'data:image/svg+xml;utf8,' +
      encodeURIComponent(canvas.toSVG()));
  };
  $(function () {
      $("#addImg").uploadPreview({ Img: "ImgPr", Width: 300, Height: 300, Callback:setImg });
  });
</script>