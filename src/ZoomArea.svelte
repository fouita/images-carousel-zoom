<script>
  export let show_box = false;
  export let src  = '' 
  export let scroll_div 
  function setScrollDiv(n) {
        scroll_div = n;
        scroll_div.scrollTop = 0;
        scroll_div.scrollLeft = 0;
  }

</script>

<style>
    .fix_wh {
      width: 1300px;
      height: 1300px;
    }
    .fix_small {
      width: 532px;
      height: 532px;
    }
</style>

<div class="hidden md:block">
  <div class="relative {show_box ? '' : 'hidden'} ">
      <div class="fixed top-0 right-0 mr-20 mt-12 z-20">
      <div
          class="fix_small overflow-hidden border-2 border-blue-300"
          use:setScrollDiv>
          <div class="fix_wh">
          <img class="fix_wh" alt="" {src} />
          </div>
      </div>
      </div>
  </div>
</div>