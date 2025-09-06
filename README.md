# Scrolling-frame-animation-with-html5-canvas


## html code

>>   <div id="page1" class="w-full  bg-black ">
            <div id="parent" class="w-full bg-zinc-600 h-[700vh]">
                   <div id="child" class="w-full h-screen  sticky top-0 left-0">
                     <canvas id="draw" class="h-screen w-full">

                     </canvas>
                   </div>
            </div>
   </div>



## javascript code


 const canvas =document.querySelector("canvas")
const context = canvas.getContext("2d")
// what ever we draw in canvas through we have to make context

const frames = {
  currentIndex : 0,
  maxIndex: 1345,
}

let imagesLoaded = 0
const images = []

function preloadImages() {

  for(let i=1;i<=frames.maxIndex;i++){
     const imageUrl = `./frames/frame_${i.toString().padStart(4,"0")}.jpeg`;
    // console.log(imageUrl);
     const img = new Image();
    img.src = imageUrl
    // console.log(img);
    img.onload = ()=>{
          imagesLoaded++;
           if(imagesLoaded === frames.maxIndex){
            // console.log("all images loaded");
            // console.log(images);
            loadImage(frames.currentIndex);
            startAnimation();
           }
    }
images.push(img)

  }


}




function loadImage(indx){

  if(indx>=0 && indx<=frames.maxIndex){
    const img = images[indx];
    canvas.width = window.innerWidth;
    canvas.height = window.innerHeight;
    
    // fit imgage with canvas with proper scale
    const scaleX = canvas.width / img.width;
    const scaleY = canvas.height / img.height;
    const scale =   Math.max(scaleX,scaleY)
    const newWidth = img.width*scale;
    const newHeight = img.height*scale;
    const offSetX = (canvas.width - newWidth) / 2;
    const offSetY = (canvas.height - newHeight) / 2 ;
    
    context.clearRect(0,0,canvas.width,canvas.height);
    context.imageSmoothingQuality="high";
    context.imageSmoothingEnabled = true;
    context.drawImage(img,offSetX,offSetY,newWidth,newHeight);
    frames.currentIndex = indx;

  }

}




function startAnimation(){
  let tl = gsap.timeline({
    scrollTrigger: {
      trigger: "#parent",
      start: "top top",
      scrub: 2,
    }
  });


  tl.to(frames,{
    currentIndex:frames.maxIndex,
    onUpdate:function(){
      loadImage(Math.floor(frames.currentIndex))
    }
  })
   
  
}





preloadImages()
