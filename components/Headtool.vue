<template>
    <div class="btn_area">
        <span class="importImg mr40">
          <input type="file" id="addImg" class="addImg" @change="setImg()">
        </span>
        <span class="mr40" @click="rasterize()"><i class="fa fa-sign-out mr10"></i>导出</span> 
        <span class="btn_item mr40" @click="drop()"><i class="fa fa-crop mr10"></i>裁剪</span>     
    </div>
	<div class="modal fade bs-example-modal-lg in" id="modalBox">
      <div class="modal-backdrop fade in" style="z-index:0;"></div>
	  <div class="modal-dialog modal-lg" style="width:1200px;height:540px;">
	    <div class="modal-content" style="width:1200px;height:540px;padding:20px 0;">
            <div class="modal-header" style="padding:0;">
              <button type="button" class="close" @click="closeModal()" style='margin:-15px 13px 10px 0;'>
                <span aria-hidden="true">×</span><span class="sr-only">Close</span>
              </button>
            </div>
	    	<div class="modal-body" id="drop_canvas" style="padding:10px 15px;"></div>
	    </div>
	  </div>
	</div>
</template>
<script>
    import DropModal from './DropModal.vue'
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
            },
            drop(){
                var canvasData = canvas.toDataURL('png');
                document.getElementById('drop_canvas').innerHTML = DropModal.template;
                document.getElementById('drop_img').innerHTML = '<img src='+ canvasData +' style="width:100%;height:100%">';
                document.getElementById('modalBox').style.display = 'block';
                this.handleDrop();
            },
            closeModal(){
                document.getElementById('modalBox').style.display = 'none';
            },
            handleDrop(){
                var $image = $('.img-container > img'),
                $dataX = $('#dataX'),
                $dataY = $('#dataY'),
                $dataHeight = $('#dataHeight'),
                $dataWidth = $('#dataWidth'),
                $dataRotate = $('#dataRotate'),
                options = {
                  aspectRatio: 16 / 9,
                  preview: '.img-preview',
                  crop: function (data) {
                    $dataX.val(Math.round(data.x));
                    $dataY.val(Math.round(data.y));
                    $dataHeight.val(Math.round(data.height));
                    $dataWidth.val(Math.round(data.width));
                    $dataRotate.val(Math.round(data.rotate));
                  }
                };

            $image.on({
              'build.cropper': function (e) {
                console.log(e.type);
              },
              'built.cropper': function (e) {
                console.log(e.type);
              },
              'dragstart.cropper': function (e) {
                console.log(e.type, e.dragType);
              },
              'dragmove.cropper': function (e) {
                console.log(e.type, e.dragType);
              },
              'dragend.cropper': function (e) {
                console.log(e.type, e.dragType);
              },
              'zoomin.cropper': function (e) {
                console.log(e.type);
              },
              'zoomout.cropper': function (e) {
                console.log(e.type);
              }
            }).cropper(options);


            // Methods
            $(document.body).on('click', '[data-method]', function () {
              var data = $(this).data(),
                  $target,
                  result;

              if (data.method) {
                data = $.extend({}, data); // Clone a new one

                if (typeof data.target !== 'undefined') {
                  $target = $(data.target);

                  if (typeof data.option === 'undefined') {
                    try {
                      data.option = JSON.parse($target.val());
                    } catch (e) {
                      console.log(e.message);
                    }
                  }
                }

                result = $image.cropper(data.method, data.option);

                if (data.method === 'getCroppedCanvas') {
                  $('#getCroppedCanvasModal').modal().find('.modal-body').html(result);
                }

                if ($.isPlainObject(result) && $target) {
                  try {
                    $target.val(JSON.stringify(result));
                  } catch (e) {
                    console.log(e.message);
                  }
                }

              }
            }).on('keydown', function (e) {

              switch (e.which) {
                case 37:
                  e.preventDefault();
                  $image.cropper('move', -1, 0);
                  break;

                case 38:
                  e.preventDefault();
                  $image.cropper('move', 0, -1);
                  break;

                case 39:
                  e.preventDefault();
                  $image.cropper('move', 1, 0);
                  break;

                case 40:
                  e.preventDefault();
                  $image.cropper('move', 0, 1);
                  break;
              }

            });


            // Import image
            var $inputImage = $('#inputImage'),
                URL = window.URL || window.webkitURL,
                blobURL;

            if (URL) {
              $inputImage.change(function () {
                var files = this.files,
                    file;

                if (files && files.length) {
                  file = files[0];

                  if (/^image\/\w+$/.test(file.type)) {
                    blobURL = URL.createObjectURL(file);
                    $image.one('built.cropper', function () {
                      URL.revokeObjectURL(blobURL); // Revoke when load complete
                    }).cropper('reset', true).cropper('replace', blobURL);
                    $inputImage.val('');
                  } else {
                    showMessage('Please choose an image file.');
                  }
                }
              });
            } else {
              $inputImage.parent().remove();
            }


            // Options
            $('.docs-options :checkbox').on('change', function () {
              var $this = $(this);

              options[$this.val()] = $this.prop('checked');
              $image.cropper('destroy').cropper(options);
            });
            }
        }
    }
</script>
