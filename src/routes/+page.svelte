<script>
// @ts-nocheck



    import { Router, Link, Route } from "svelte-navigator";
    import Footer from "../components/Footer.svelte";
     import "../app.css";
	import Navbar from "../components/Navbar.svelte";
  import { writable, derived } from "svelte/store";
  import {browser} from '$app/environment'
  // import {browser} from '/env'
	// import Products from "../routes/products/+page.svelte";

export let data
// console.log(data?.products, 'datatata home')


export const cartDetails = writable({
  item: []
})

</script>



	<div>

		<Navbar/>



        <div class='mt-40'>
  <div class='flex'>
    <div class='flex-grow text-4xl font-extrabold text-center'>Special Qualities For You</div>
  </div>
  <div class='max-w-12xl mx-auto h-full flex flex-wrap justify-center py-28 gap-10'>
    {#each data?.products as x, i}
      <div  class=''>
        <div on:click={() => console.log(`/singleproduct/${x.id}`, { state: { id: x.id } })} on:keydown|preventDefault={(event) =>
          event.key === "Enter" || (event.key === " " && console.log("/home"))} class='rounded-lg shadow-lg bg-white max-w-sm'>
          <a href={`/singlepage/${x.id}`} data-mdb-ripple='true' data-sveltekit-prefetch data-mdb-ripple-color='light'>
            <img class='rounded-t-lg' src='{x.thumbnail}' alt='' />
          </a>
          <div class='bg-purple-100 py-8 relative font-bold text-xl w-full flex flex-col justify-center px-6'>
            <div class=''>{x.title}</div>
            <div class=''>
              &euro; {x.variants[0].prices[0].amount / 100}
            </div>
          </div>
        </div>
      </div>

    {/each}
  </div>
</div>





        <Footer/>


	</div>









