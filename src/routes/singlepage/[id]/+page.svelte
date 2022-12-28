<script>
  // @ts-nocheck
  import "../../../app.css";
  import "../../../components/Navbar.svelte";
  import axios from "axios";
  import Navbar from "../../../components/Navbar.svelte";

  export let data;

  let currentImg = 0;
  let imgArr = [];
  let productData = [];
  let product = [];
  let sizes = [];
  let currentSize = "S";
  let priceList = [];
  let currentPrice = "";
  let variantsId = 0;
  let products = [];
  let notify = "-right-64";
   let cartId = "";

  import { writable, derived } from "svelte/store";
  import {browser} from '$app/environment'


  export const cartDetails = writable({
  cart_id: '',
  // cartItem: []
})



  const fetchData = async () => {
    // window.scrollTo(0, 0);
        cartId = browser && localStorage.getItem('cart_id')

    try {
      const response = await axios.get(
        `http://localhost:9000/store/products/${data.params.id}`
      );
      // const response = await axios.get(`http://localhost:9000/store/products/prod_01GN6666V4R3KWPPTJ0GMD6TD4`);

      // console.log(response, 'response')
      // console.log(localStorage.getItem("cart_id", "cartiddddddddddd"));
      productData = response?.data;
      product = response?.data?.product;
      imgArr = response?.data?.product?.images;
      sizes = response?.data?.product?.variants?.map(
        (variant) => variant.title.split("/")[0]
      );
      priceList = response?.data?.product?.variants?.map(
        (x) => x.prices[0]?.amount
      );
      currentPrice = response?.data?.product?.variants?.map(
        (x) => x.prices[0]?.amount
      )[0];

    //  cartId = localStorage?.cart_id;
    // console.log(localStorage.getItem('cart_id'), 'localstorage')



      if (!cartId) {
        const config = {
          method: "post",
          url: "http://localhost:9000/store/carts",
          headers: {
            Cookie:
              "connect.sid=s%3ABtOTTvAMucVXI50x7QfBxVs_htUl9tH0.qVmwCkwQ1N2kv9KsjAwB5LZq7NKeWaQxnmC%2FGkToOTA",
          },
        };

        try {
          const response = await axios(config);
          console.log((response?.data?.cart_id, "save this to compare"));
          localStorage.cart_id = response?.data?.cart?.id;
        } catch (error) {
          console.log(error);
        }
      }
    } catch (err) {
      console.log(err, "err");
    }
  };

  // $: data.params.id;

  $: fetchData();


  const addProduct = (data) => {
    console.log(data);
    cartId = browser && localStorage.cart_id;
    console.log(browser &&  localStorage.cart_id, "localStorage.cart_id");
    console.log(cartId, 'cartId')
    console.log(currentSize, 'sizessss')
    axios
      .post(
        `http://localhost:9000/store/carts/${localStorage.cart_id}/line-items`,
        {
          variant_id: data,
          quantity: 1,
          metadata: {
            size: currentSize
          }
        }
      )
      .then(({ data }) => {
        console.log(data, "datatatat  ----compare the cart_iddd");

        products = [...products, data?.cart];

        localStorage.cart_1 = products;
        localStorage.setItem("cart", JSON.stringify([...products]));

        console.log(([...products]), 'derftgyhujik')
        console.log(JSON.parse( browser && localStorage.getItem('cart')), 'gyguyguygyuguy')
        notify = "right-3";
        const interval = setInterval(() => {
          notify = "-right-64";
          clearInterval(interval);
        }, 1000);
        window.location.reload()
      });
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
                  <img src={imgArr[currentImg]?.url} alt="no image_" />
                  <div class="absolute overflow-x-scroll w-full bottom-0 right-0 p-4 flex flex-nowrap gap-4">
                    <div class="flex w-full flex-nowrap gap-4 rounded-lg">
                      {#each imgArr as img, i}
                        <div key={i}   on:keydown|preventDefault={(event) =>
                    event.key === "Enter" ||
                   (event.key === " " && console.log("/products"))}
                   on:click={() => currentImg = i}
                   title={imgArr[i]?.url}
                   class="w-16 h-24 flex-none">
                          <div class="h-full w-full rounded-lg cursor-pointer shadow-lg border overflow-hidden">
                            <img src={imgArr[i]?.url} alt="" class="h-full w-full" />
                          </div>
                        </div>
                      {/each}
                    </div>
                  </div>
                </div>
              </div>
            </div>
          </div>
          <div>
            <div class="flex md:flex-col flex-col space-y-7 justify-center">
              <div class="text-black space-y-3">
                <h2 class="font-bold text-xl text-black">{product.title}</h2>
                <p class="text-sm">{product.description}</p>
              </div>
              <div class="space-y-3">
                <div class="font-bold text-md text-black">Select Size</div>
                <div class="flex flex-row flex-wrap gap-4">
                  {#each sizes as size, i}
                    <div>
                      <div
                        on:click={() => {
                          // setCurrentSize(size);
                          currentSize = size
                          currentPrice = priceList[i]
                          // setCurrentPrice(priceList[i]);

                          variantsId = product.variants[i].id
                          // setVariantsId(product.variants[i].id);
                        }}
                          on:keydown|preventDefault={(event) =>
              event.key === "Enter" ||
              (event.key === " " && console.log("/products"))}
                        class={currentSize === size ? 'border-purple-300 bg-purple-100' : 'border-gray-100'}
                        contenteditable={false}
                      >
                        <span class="text-black text-sm">{size}</span>
                      </div>
                    </div>
                  {/each}
                </div>
              </div>
              <div class="space-y-3">
                <div class="font-bold text-md text-black">Price</div>
                <div class="text-black">${currentPrice}</div>
              </div>
              <button class="bg-purple-700 text-white font-bold py-2 px-4 rounded-full" on:click={() => addProduct(product.variants[0]?.id)}
     on:keydown|preventDefault={(event) =>
              event.key === "Enter" ||
              (event.key === " " && console.log("/products"))}>
                Add to Cart
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>





 </main>
</div>
