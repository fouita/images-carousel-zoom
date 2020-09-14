<script>
    import ZoomImage from './ZoomImage.svelte'
    import ZoomArea from './ZoomArea.svelte'
    import ZoomIcon from './ZoomIcon.svelte'
    export let images 
    let picture

    let border_show = (new Array(6)).map((e,i) => i==0)

    function showHideborder(i) {
        border_show = [false, false, false, false, false, false];
        border_show[i] = true;
    }

    function changeImage(pic, i) {
        picture.scrollLeft = i * picture.offsetWidth;
        images.selected = pic;

    }

    function setPicture(n) {
        picture = n;
        picture.scrollLeft;
    }
    
</script>

<style>
    .o_img_fix {
      width: 400px;
      height: 400px;
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
      {#each images.list as l,i}
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
      <div class="o_img_fix relative"> 
        <div class="flex overflow-hidden scroll o_img_fix" use:setPicture>
            {#each images.list as src,i}
                <ZoomImage {src} />
            {/each}
        </div>
      </div>
        <div class="items-center justify-center mt-2 hidden md:flex">
        <ZoomIcon />
        <div class="text-xs text-gray-700 font-semibold ml-2">
          Roll over image to zoom in
        </div>
      </div>
    </div>
  </div>
</div>

