<script>
// @ts-nocheck

	import Navbar from '../../components/Navbar.svelte';

// @ts-nocheck

import '../../app.css'
export let data
console.log(data?.products, 'datatata')


</script>



<Navbar/>

<div class='mt-40'>
  <div class='flex'>
    <div class='flex-grow text-4xl font-extrabold text-center'>Best Qualities You Can Trust</div>
  </div>
  <div class='max-w-12xl mx-auto h-full flex flex-wrap justify-center py-28 gap-10'>
    {#each data?.products as x, i}
      <div  class=''>
        <div on:click={() => console.log(`/singleproduct/${x?.id}`, { state: { id: x.id } })} on:keydown|preventDefault={(event) =>
          event.key === "Enter" || (event.key === " " && console.log("/home"))} class='rounded-lg shadow-lg bg-white max-w-sm'>
          <!-- <a href='#!' data-mdb-ripple='true' data-mdb-ripple-color='light'>
            <img class='rounded-t-lg' src='{x.thumbnail}' alt='' />
          </a> -->
           <a href={`/singlepage/${x?.id}`} data-mdb-ripple='true' data-sveltekit-prefetch data-mdb-ripple-color='light'>
            <img class='rounded-t-lg' src='{x.thumbnail}' alt='' />
          </a>
          <div class='bg-red-400 py-8 relative font-bold text-xl w-full flex flex-col justify-center px-6'>
            <div class=''>{x.title}</div>
            <div class=''>
              &euro; {x?.variants[0]?.prices[0]?.amount / 100}
            </div>
          </div>
        </div>
      </div>

    {/each}
  </div>
</div>
