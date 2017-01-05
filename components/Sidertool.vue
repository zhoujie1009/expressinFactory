<template>
	<div class="sider_bar">
      <span class="bar_item bar_active importImg">
        <input type="file" id="addImg" class="addImg" @change="setImg()">
      </span>
	    <span class="bar_item" @click="rasterize()"><i class="fa fa-sign-out mr18"></i>导出</span>
	    <span class="bar_item"><i class="fa fa-floppy-o mr18"></i></i>保存</span>
	</div>
</template>
<script>
    export default{
        data(){
            return {

            }
        },
        methods:{
            setImg(){
                var imgUrl = this.getObjectURL(document.getElementById('addImg').files[0]);
                var randomVal1 = Math.random();
                var randomVal2 = Math.random();
                this.addImage(imgUrl,Math.min(randomVal1, randomVal2),Math.max(randomVal1, randomVal2));
            },
            getObjectURL(file) {
                var url = null;
                if (window.createObjectURL != undefined) {
                    url = window.createObjectURL(file)
                } else if (window.URL != undefined) {
                    url = window.URL.createObjectURL(file)
                } else if (window.webkitURL != undefined) {
                    url = window.webkitURL.createObjectURL(file)
                }
                return url
            },
            addImage(imgAdd, minScale, maxScale) {
                var _this = this;
                var coord = this.getRandomLeftTop();
                fabric.Image.fromURL(imgAdd, function(image) {
                  image.set({
                    left: coord.left,
                    top: coord.top,
                    type: 'image',
                    angle: fabric.util.getRandomInt(-10, 10)
                  })
                  .scale(_this.getRandomNum(minScale, maxScale))
                  .setCoords();
                  canvas.add(image);
                });
                this.uploadTime = 0;
            },
            getRandomNum(min, max) {
                return Math.random() * (max - min) + min;
            },
            getRandomLeftTop() {
                var offset = 50;
                return {
                    left: fabric.util.getRandomInt(0 + offset, 700 - offset),
                    top: fabric.util.getRandomInt(0 + offset, 500 - offset)
                };
            },
            rasterize() {
                if (!fabric.Canvas.supports('toDataURL')) {
                    alert('This browser doesn\'t provide means to serialize canvas to an image');
                }else {
                    window.open(canvas.toDataURL('png'));
                }
            },
            rasterizeSVG() {
                window.open(
                  'data:image/svg+xml;utf8,' +
                  encodeURIComponent(canvas.toSVG()));
            }
        }
    }
</script>
