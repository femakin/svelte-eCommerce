<script>
  // @ts-nocheck

  import Footer from "../components/Footer.svelte";
  import "../app.css";
  import Navbar from "../components/Navbar.svelte";

  import {sharedLoad} from '../util/shared';

  let products;
    const loadData = async () => {
        const data = await sharedLoad({ fetch });
        products = data;
    }

  $: loadData()
</script>

<div>
  <Navbar />
   <div class="mt-40">
    <div class="flex">
      <div class="flex-grow text-4xl font-extrabold text-center">
        Best Qualities You Can Trust
      </div>
    </div>
    <div class="flex">
      <div class="flex-grow text-4xl mt-12 font-extrabold text-center">
        <a
          href="/products"
          class="bg-red-400 hover:bg-red-700 text-white font-bold py-2 px-4 rounded "
          >Products</a
        >
      </div>
    </div>
    <div
      class="max-w-12xl mx-auto h-full flex flex-wrap justify-center py-28 gap-10"
    >
    {#if products}
      {#each products?.products as product, i}
        <div class="">
          <div

            class="rounded-lg shadow-lg bg-white max-w-sm"
          >
            <a
              href={`/products/${product.id}`}
              data-mdb-ripple="true"
              data-sveltekit-prefetch
              data-mdb-ripple-color="light"
            >
              <img class="rounded-t-lg" src={product.thumbnail} alt="" />
            </a>
            <div
              class="bg-red-400 py-8 relative font-bold text-gray-100 text-xl w-full flex flex-col justify-center px-6"
            >
              <div class="">{product.title}</div>
              <div class="">
                &euro; {product.variants[0].prices[0].amount / 100}
              </div>
            </div>
          </div>
        </div>
      {/each}
      {/if}
    </div>
  </div>
  <Footer />
</div>
