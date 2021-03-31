<script>

    export let showDetailsModal = false;
    export let product;
    let productFromApi;

</script>
  
{#if showDetailsModal}
    <div class="modal is-active">
        <div class="modal-background" on:click|self></div>
        <div class="modal-card">
        <header class="modal-card-head has-text-centered">
            <p class="modal-card-title">Product Details</p>
            <button on:click class="delete" aria-label="close"></button>
        </header>
        <section class="modal-card-body">
            <p class="is-size-2">ID: {product.id}</p>
            <p class="is-size-2">Name: {product.name}</p>
            {#if product.inStock == true }
                <span class="is-size-2">In Stock: </span><span class = "has-text-success is-size-2">Available</span>
            {:else}
                <span class="is-size-2">In Stock: </span><span class = "has-text-danger is-size-2">Unavailable</span>
            {/if}
        </section>
            <footer class="modal-card-foot has-text-centered">
                <div class="content has-text-centered">
                    <button class="button is-danger" on:click>Cancel</button>
                </div>                
            </footer>
        </div>
    </div>
{/if}

