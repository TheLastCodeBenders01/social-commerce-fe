<script lang="ts">
    import IconShoppingCart from "@tabler/icons-svelte/icons/shopping-cart";
    import { getProductById, type Product, Loader } from "$lib";

    let carts: number[] = $state(
        JSON.parse(localStorage.getItem("carts") || "[]"),
    );
    let products: Promise<Product[]> = $derived(
        Promise.all(carts.map((cart) => getProductById(cart.toString()))),
    );
    let isVisible: boolean = $state(false);

        function onclick(){
            isVisible = !isVisible;
            carts = JSON.parse(
                localStorage.getItem("carts") || "[]",
            );
        }
</script>

<button
    class="transform hover:scale-120 transition relative"
    {onclick}
>
    <IconShoppingCart class="stroke-blue-700 size-8 hover:stroke-blue-500 " />
</button>

<div
    class="absolute right-10 md:right-16 top-10 md:top-14 bg-slate-100 border-1 border-slate-200 shadow-lg rounded-lg p-4 w-64 {isVisible
        ? 'visible'
        : 'hidden'}"
>
    {#await products}
        <div class="w-full justify-items-center">
            <Loader --color="#3b82f6" --width="25px" />
        </div>
    {:then data}
        {#if data.length > 0}
            <h2 class="text-lg font-semibold mb-2">Shopping Cart</h2>
            <ul class="divide-y divide-gray-200">
                {#each data as product}
                    <li class="flex items-center justify-between mb-2">
                        <div class="flex items-center">
                            <span>{product.name}</span>
                        </div>
                        <span class="font-semibold">{product.amount}</span>
                    </li>
                {/each}
            </ul>
        {:else}
            <p>No items in cart</p>
        {/if}
    {/await}
</div>

<style>
    .hidden {
        display: none;
    }

    .visible {
        display: block;
    }
</style>
