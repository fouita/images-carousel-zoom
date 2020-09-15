<script>
    import Carousel from './Carousel.svelte'
    
    
  export const images ={
    selected :"https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021/13021743.jpg",
	list :  [
    
	 "https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021/13021743.jpg",
	 "https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021/13021743_1.jpg",
     "https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021/13021743_2.jpg",
     "https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021/13021743_3.jpg",
     "https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021/13021743_4.jpg",
	]
   } 


</script>

<Carousel {images} />