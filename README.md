# HTML5Canvas

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Canvas Animation</title>
    <script src="https://cdn.tailwindcss.com"></script>
</head>

<body>
    
    <div class="w-full bg-zinc-900">
        <div class="parent relative top-0 left-0 w-full h-[1500vh]">
            <div class="w-full sticky top-0 left-0 h-screen">
                <canvas class="w-full h-screen" id="frame"></canvas>
            </div>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/gsap.min.js" integrity="sha512-7eHRwcbYkK4d9g/6tD/mhkf++eoTHwpNM9woBxtPUBWm67zeAfFC+HrdoE2GanKeocly/VxeLvIqwvCdk7qScg==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.5/ScrollTrigger.min.js" integrity="sha512-onMTRKJBKz8M1TnqqDuGBlowlH0ohFzMXYRNebz+yOcc5TQr/zAKsthzhuv0hiyUKEiQEQXEynnXCvNTOk50dg==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>
    <script>
        const canvas = document.querySelector("canvas");
        const context = canvas.getContext("2d");

        const frames = {
            currentIndex:0,
            maxIndex: 382
        }

        let imagesLoaded = 0;
        const images = [];

        function preloadImages(){
            for(var i=1; i<=frames.maxIndex; i++){
                const imageUrl = `./dawood/frame_${i.toString().padStart(4, "0")}.jpeg`;
                const img = new Image();
                img.src = imageUrl;
                img.onload = () => {
                      imagesLoaded++;
                      if(imagesLoaded === frames.maxIndex){
                           loadImage(frames.currentIndex);
                           startAnimation();
                      }
                }
                images.push(img);
            }
        }
        
        function loadImage(index){
             if(index>=0 && index<=frames.maxIndex){
                const img = images[index];
                canvas.width = window.innerWidth;
                canvas.height = window.innerHeight;

                const scaleX = canvas.width / img.width;
                const scaleY = canvas.height / img.height;
                const scale = Math.max(scaleX, scaleY);

                const newWidth = img.width * scale;
                const newHeight = img.height * scale;

                const offsetX = (canvas.width - newWidth) / 2;
                const offsetY = (canvas.height - newHeight) / 2;

                context.clearRect(0, 0, canvas.width, canvas.height);
                context.imageSmoothingEnabled = true;
                context.imageSmoothingQuality = "high";
                context.drawImage(img, offsetX, offsetY, newWidth, newHeight);
                frames.currentIndex = index;
             }
        }
        
        function startAnimation(){
           
           var tl = gsap.timeline({
               scrollTrigger:{
                  trigger: ".parent",
                  start: "top top",
                  scrub: 2,
                  end: "bottom bottom",
               }
           })

           tl.to(frames, {
               currentIndex: frames.maxIndex,
               onUpdate: function(){
                  loadImage(Math.floor(frames.currentIndex));
               }
            })
        }
        preloadImages();
    </script>
</body>
</html>
