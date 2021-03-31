<script>

    import { createEventDispatcher } from "svelte";

    let dispatch = createEventDispatcher();

    let name;
	  let inStock;

    const handleSubmit = () => {
            
        const product = {
            name: name,
            inStock: (inStock == "Yes") ? true : false
        };

        dispatch('addProduct', product);        
    };
</script>

<form on:submit={handleSubmit}>


    <div class="field">
        <label class="name" for="name">Name</label>
        <div class="control">
            <input name="name" class="input is-primary is-fullwidth" type="text" bind:value={name}>
        </div>
      </div>
    
      <div class="field">
        <label class="label" for="instock">In Stock</label>
        <div class="control">
          <div class="select is-fullwidth is-primary" name="instock">
            <select bind:value={inStock}>
              <option>Yes</option>
              <option>No</option>
            </select>
          </div>
        </div>
      </div>

      <div class="field is-grouped">
        <p class="control">
          <button class="button is-primary">
            Submit
          </button>
        </p>
        <p class="control">
          <!-- svelte-ignore a11y-missing-attribute -->
          <a on:click class="button is-light">
            Cancel
          </a>
        </p>
      </div>
</form>
 
