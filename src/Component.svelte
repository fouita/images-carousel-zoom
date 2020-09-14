<script>
import { each } from "svelte/internal";
import Zoom from "./ZoomImage.svelte";
import ZoomIcon from "./ZoomIcon.svelte";
import Carousel from "./Carousel.svelte";
  let m = { x: 0, y: 0 };
  let node = null;
  let container = null;
  let scrollDiv = null;
  let show_box = false;
  let border_show = [true, false, false, false, false, false];
  let box = null;
  let picture = null;
  let picture_small = null;

  function setCoords() {
    box = container.getBoundingClientRect();
    if (
      m.x + node.clientWidth / 2 <= box.x + box.width &&
      m.y + node.clientHeight / 2 <= box.y + box.height &&
      m.x - (box.x + node.clientWidth / 2) >= 0 &&
      m.y - (box.y + node.clientHeight / 2) >= 0
    ) {
      node.style.top = -box.y + m.y + "px";
	  node.style.left = -box.x + m.x + "px";
    } else {
      if (m.x < (box.x + node.clientWidth / 2)) {
        node.style.left = node.clientWidth / 2 + "px";
		node.style.top = -box.y + m.y + "px";
      }

      if (m.x > box.x + box.width - node.clientWidth / 2) {
        node.style.left = box.width - node.clientWidth / 2 + "px";
        node.style.top = -box.y + m.y + "px";
      }

      if (m.y > box.y + box.height - node.clientHeight / 2) {
        node.style.top = box.height - node.clientHeight / 2 + "px";
        if (
          m.x < box.x + box.width - node.clientWidth / 2 &&
          m.x > box.x + node.clientWidth / 2
        ) {
          node.style.left = -box.x + m.x + "px";
        }
      }

      if (m.y < box.y + node.clientHeight / 2) {
        node.style.top = node.clientHeight / 2 + "px";
        if (
          m.x < box.x + box.width - node.clientWidth / 2 &&
          m.x > box.x + node.clientWidth / 2
        ) {
          node.style.left = -box.x + m.x + "px";
        }
      }
    }

	  show_box = true;
	
  }
  function handleMousemove(event) {
    m.x = event.clientX;
    m.y = event.clientY;
    setCoords();
    scrollTop();
  }
  function setNode(n) {
    node = n;
  }
  function setContainer(n) {
    container = n;
  }
  function setScrollDiv(n) {
    scrollDiv = n;
    scrollDiv.scrollTop = 0;
    scrollDiv.scrollLeft = 0;
  }

  function cancelMouseMove() {
    show_box = false;
  }

  function scrollTop() {
    box = container.getBoundingClientRect();
    scrollDiv.scrollTop =
      (Number(node.style.top.replace("px", "")) - node.clientHeight / 1.8) *
        3.25 -
      container.offsetTop / 3.25;
    scrollDiv.scrollLeft =
      (Number(node.style.left.replace("px", "")) - node.clientWidth / 2.25) *
        3.25 -
      container.offsetLeft / 3.25;
  }

  export const images ={
    selected :"https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021/13021743.jpg",
	List :  [
	 "https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021/13021743.jpg",
	 "https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021/13021743_1.jpg",
     "https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021/13021743_2.jpg",
     "https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021/13021743_3.jpg",
     "https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021/13021743_4.jpg",
	]
   } 

  function showHideborder(i) {
    border_show = [false, false, false, false, false, false];
    border_show[i] = true;
  }

  function setPicture(n) {
    picture = n;
    picture.scrollLeft;
  }

  function changeImage(pic, i) {
    picture.scrollLeft = i * picture.offsetWidth;
    images.selected = pic;
  }
  let imagesList = images.List;
</script>

<style>
  .o_img_fix {
    width: 400px;
    height: 400px;
  }
  .fix_wh {
    width: 1300px;
    height: 1300px;
  }
  .fix_small {
    width: 532px;
    height: 532px;
  }
  .scroll {
    scroll-behavior: smooth;
  }
</style>

<div
  class="lg:flex items-center flex-auto bg-white max-w-screen-xl mx-auto
    relative">
  <div
    class="sm:block md:flex items-center sm:justify-center md:justify-start
      md:mr-4 lg:justify-center bg-white">
      
    <div class="flex md:block items-center justify-center bg-white">
      {#each images.List as l,i}
        <div class="mt-2 mr-2 ml-2">
          <div
            class="p-1 cursor-pointer rounded {border_show[i] ? 'border-2' : ''} border-blue-300"
            on:click={() => showHideborder(i)} on:click={() => changeImage(l, i)}>
            <button
              class="h-16 w-16 focus:outline-none">
              <img alt="" src={l} />
            </button>
          </div>
        </div>
      {/each}
    </div>

    <div class="xl:ml-10">
      <div class="flex justify-center items-center">
        <div
          class="o_img_fix relative"
          on:mousemove={handleMousemove}
          on:mouseleave={cancelMouseMove}
          use:setContainer>
          <div class="absolute flex overflow-x-hidden scroll o_img_fix" use:setPicture>
            <Carousel {imagesList}/>
          </div>
          <div class="hidden md:block">
            <div
              id="follow_mouse"
              class="{show_box ? '' : 'hidden'} bg-blue-200 border-4 border-blue-300
                h-40 w-40 -mt-20 -ml-20 absolute opacity-50 rounded"
              use:setNode />
          </div>
        </div>
      </div>
      <div class="items-center justify-center mt-2 hidden md:flex">
        <ZoomIcon />
        <div class="text-xs text-gray-700 font-semibold ml-2">
          Roll over image to zoom in
        </div>
      </div>
    </div>
    <div class="hidden md:block">
      <div class="relative {show_box ? '' : 'hidden'} px-6">
        <div class="fixed top-0 mt-12 z-20">
          <div
            class="fix_small overflow-hidden border-2 border-blue-300"
            use:setScrollDiv>
            <div class="fix_wh">
              <img class="fix_wh" alt="" src={images.selected} />
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
