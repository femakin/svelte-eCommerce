<script>
  // @ts-nocheck

  import Navbar from "../../components/Navbar.svelte";

  // @ts-nocheck

  import "../../app.css";
	import { getProducts } from "../../util/shared";



    let products;
    const productData = async () => {
        const data = await getProducts();
        products = data;
        console.log(products, 'products')
    }

  $: productData()
</script>

<Navbar />

<div class="mt-40">
  <div class="flex">
    <div class="flex-grow text-4xl font-extrabold text-center">
      Best Qualities You Can Trust
    </div>
  </div>
  <div
    class="max-w-12xl mx-auto h-full flex flex-wrap justify-center py-28 gap-10"
  >

  {#each products?.products as product, i}
      <div class="">
        <div

          class="rounded-lg shadow-lg bg-white max-w-sm"
        >
          <a
            href={`/products/${product?.id}`}
            data-mdb-ripple="true"
            data-sveltekit-prefetch
            data-mdb-ripple-color="light"
          >
            <img class="rounded-t-lg" src={product?.thumbnail} alt="" />
          </a>
          <div
            class="bg-red-400 py-8 relative font-bold text-gray-100 text-xl w-full flex flex-col justify-center px-6"
          >
            <div class="">{product?.title}</div>
            <div class="">
              &euro; {product?.variants[0]?.prices[0]?.amount / 100}
            </div>
          </div>
        </div>
      </div>
    {/each}
  </div>
</div>