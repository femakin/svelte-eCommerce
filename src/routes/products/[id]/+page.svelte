<script>
  // @ts-nocheck
  import "../../../app.css";
  import "../../../components/Navbar.svelte";
  import axios from "axios";
  import Navbar from "../../../components/Navbar.svelte";
  import Swal from 'sweetalert2'
  export let data;

  let responseData = [];
  let currentImg = 0;
  let currentSize = "S";
  let currentPrice = "";
  let variantsId = 0;
  let cartId = "";
  let variantTitle;
  let products = [];
  import { writable, derived } from "svelte/store";
  import {browser} from '$app/environment'

  export const cartDetails = writable({
  cart_id: '',
})


if (!cartId) {
    axios({
        method: 'post',
        url:  `${import.meta.env.VITE_API_BASE_URL}/store/carts`,
        withCredentials: true
    })
    .then(response => {
        console.log(response.data.cart.id, 'response.data.cart.id')
        localStorage.setItem("cart_id", response.data.cart.id)
    })
    .catch(error => {
        console.log(error);
    });
}

  const fetchData = async () => {
       cartId = browser && localStorage.getItem('cart_id')
    axios
      .get(`${import.meta.env.VITE_API_BASE_URL}/store/products/${data.params.id}`)
      .then((response) => {
        if (response.data.product) {
            responseData = response?.data
        }
      })
      .catch((err) => {
        console.log("error", err)
      });
  };

$: fetchData();





 const addProduct = async (data) => {
    cartId = browser && localStorage.cart_id;

    try {
        const response = await axios.post(
            `${import.meta.env.VITE_API_BASE_URL}/store/carts/${localStorage.cart_id}/line-items`,
            {
                variant_id: data,
                quantity: 1,
                metadata: {
                    size: currentSize
                }
            }
        );

        products = [response.data?.cart];
         browser && localStorage.setItem("cart", JSON.stringify([products]));
            if (response?.data?.cart) {
                if (response?.status === 200) {
              Swal.fire({
            icon: 'success',
            title: 'Item Added to Cart Successfully',
            showConfirmButton: true,
        }).then((res) => {

           if (res.isConfirmed) {
     window.location.reload();
  }

        });
                }
            }

    } catch (error) {
        console.log(error);
    }
};



</script>


<div class="mt-40">
  <main>
  <Navbar productId={JSON.parse( browser && localStorage.getItem('cart'))} />
    <div class="py-20 px-4">
      <div class="text-white max-w-6xl mx-auto py-2">
        <div class="grid md:grid-cols-2 gap-20 grid-cols-1">
          <div>
            <div class="relative">
              <div>
                <div class="relative">

                  <img src={responseData.product?.images[currentImg]?.url} alt="no image_" />
                  <div class="absolute overflow-x-scroll w-full bottom-0 right-0 p-4 flex flex-nowrap gap-4">
                    <div class="flex w-full flex-nowrap gap-4 rounded-lg">

                    {#if responseData?.product?.images}
                    {#each responseData.product.images as img, i}

                        <div
                        key={i}
                        on:click={() => (currentImg = i)}
                        title={responseData.product.images[i].url}
                        class="w-16 h-24 flex-none"
                        on:keydown={() => (currentImg = i)}
                        >
                        <div
                            class="h-full w-full rounded-lg cursor-pointer shadow-lg border overflow-hidden"
                        >
                            <img
                            src={responseData.product.images[i].url}
                            alt=""
                            class="h-full w-full"
                            />
                        </div>
                        </div>
                    {/each}
                    {/if}
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div>
            <div class="flex md:flex-col flex-col space-y-7 justify-center">
              <div class="text-black space-y-3">
                <h2 class="font-bold text-xl text-black">{responseData?.product?.title}</h2>
                <p class="text-sm">{responseData?.product?.description}</p>
              </div>
              <div class="space-y-3">
                <div class="font-bold text-md text-black">Select Size</div>
                <div class="flex flex-row flex-wrap gap-4">
                {#if responseData?.product?.variants}
                    {#each responseData?.product?.variants as variant, i}
                    { variantTitle = variant.title.split("/")[0]}
                      <div>
                      <div
                        on:click={() => {
                           currentSize = variant?.title?.split("/")[0]
                           currentPrice = variant?.prices[0]?.amount[i]
                           variantsId = variant.id
                        }}
                        on:keydown={() => {
                           currentSize = variant?.title?.split("/")[0]
                           currentPrice = variant?.prices[0]?.amount[i]
                           variantsId = variant.id
                        }}

                        class={currentSize === variant?.title?.split("/")[0] ? 'border-purple-300 bg-red-400' : 'border-gray-100'}
                        contenteditable={false}
                      >
                        <span class="text-black text-sm">{variant?.title?.split("/")[0]}</span>
                      </div>
                    </div>
                    {/each}
                {/if}
                </div>
              </div>
              <div class="space-y-3">
                <div class="font-bold text-md text-black">Price</div>

                    {#if responseData?.product?.variants}
                        <div class="text-black">${responseData?.product?.variants.map(x => x.prices[0]?.amount)[0]}</div>
                    {/if}

              </div>

               <button
         class="bg-red-400 text-white font-bold py-2 px-4 rounded-full"
         on:click={() => {
            if (variantsId === 0) {
              alert("Please select a size before adding to cart.");
            } else {
              addProduct(variantsId);
            }
         }}
       >
         Add to Cart
       </button>
            </div>
          </div>
        </div>
      </div>
    </div>
 </main>
</div>




<!--

-->