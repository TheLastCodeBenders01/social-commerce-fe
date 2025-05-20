<script lang="ts">
    import type {Snippet} from "svelte";

    let {children, open = $bindable()}: {children: Snippet, open: boolean} = $props();
    let isOpen = $state(open);

    $effect(function (){
        if (isOpen){
            setTimeout(() => {isOpen = false}, 2000)
        }
    })   
</script>

<!--* A basic modal in tailwindcss  -->
<div class="fixed z-3 bottom-4 w-sm right-2 bg-slate-100 border-slate-200 border-1 p-4 rounded-md shadow-lg xl:bottom-20 xl:right-6 {isOpen ? "visible" : "hidden"}">
    {@render (children())}
</div>

<style> 
.hidden {
    display: none;
}

.visible {
    display: block;
}
</style>    