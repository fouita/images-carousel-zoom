<script>

  import ZoomArea from './ZoomArea.svelte'
  export let src = ''

  let container = null;
  let show_box = false;
  let scroll_div = null;
  let box = null;
  let node = null;

  let m = { x: 0, y: 0 };


  function setContainer(n) {
    container = n;
  }

  function setCoords() {
    if(!node) return
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

  function cancelMouseMove() {
    show_box = false;
  }

  function setNode(n) {
    node = n;
  }

  
  function scrollTop() {
    if(!node) return
    box = container.getBoundingClientRect();
    scroll_div.scrollTop =
      (Number(node.style.top.replace("px", "")) - node.clientHeight / 1.8) *
        3.25 - container.offsetTop / 3.25;
    scroll_div.scrollLeft =
      (Number(node.style.left.replace("px", "")) - node.clientWidth / 2.25) *
        3.25// - container.offsetLeft / 3.25;

  }


</script>

<style>
  .o_img_fix {
    width: 400px;
    height: 400px;
  }
</style>


<div
    class="flex-shrink-0 o_img_fix relative"
    on:mousemove={handleMousemove}
    on:mouseleave={cancelMouseMove}
    on:mouseenter={() => show_box= true}
    on:mouseleave={() => show_box= false}
    use:setContainer>
    
    <img {src} alt='img' class="w-full" />
    
    {#if show_box}
    <div class="hidden md:block">
        <div
            id="follow_mouse"
            class="bg-blue-200 border-4 border-blue-300
            h-40 w-40 -mt-20 -ml-20 absolute opacity-50 rounded"
            use:setNode ></div>
    </div>
    {/if}
</div>

<ZoomArea {show_box} bind:scroll_div={scroll_div} {src} />


