# Svelte + tailwindcss image preview with zoom

Svelte component to display and zoom on product images

# install
```bash
$npm install @fouita/images-carousel-zoom
```

![fouita simple zoom](https://cdn.fouita.com/assets/pics/template/image-zoom/carousel-zoom.gif)

# Responsive display

![fouita simple zoom](https://cdn.fouita.com/assets/pics/template/image-zoom/small-screen.jpg)

# How to use?

```html
<script>
    import ImgZoom from '@fouita/images-carousel-zoom'
    const base_url = "https://multimedia.bbycastatic.ca/multimedia/products/500x500/130/13021"
</script>

<ImgZoom images ={
    selected :`${base_url}/13021743.jpg`,
	List :  [
	 `${base_url}/13021743.jpg`,
	 `${base_url}/13021743_1.jpg`,
     `${base_url}/13021743_2.jpg`,
     `${base_url}/13021743_3.jpg`,
     `${base_url}/13021743_4.jpg`
	]
   } >
```

# Requirements
* Tailwindcss 
* Svelte project

# About
[Fouita : UI framework for svelte + tailwind components](https://fouita.com)

