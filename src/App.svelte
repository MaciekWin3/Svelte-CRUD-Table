<script>

	import { onMount } from "svelte";
	import Footer from "./components/Footer.svelte";
	import CreateModal from "./components/modals/AddModal.svelte";
	import AddProductForm from "./components/forms/AddProductForm.svelte";
	import EditModal from "./components/modals/EditModal.svelte";
	import EditProductForm from "./components/forms/EditProductForm.svelte";
	import ProductDetails from "./components/modals/ProductDetails.svelte";

	let showCreateModal = false;
	let showEditModal = false;
	let showDetailsModal = false;
	let editedProduct;
	let detailsProduct;


	let products = [];

	onMount( async () => {
		const res = await fetch(`https://localhost:44364/api/products`);
		products = await res.json();
  	});

	const switchCreateModal = () => {
		showCreateModal = !showCreateModal;
	}

	const switchEditModal = () => {
		showEditModal = !showEditModal;
	}

	const switchDetailsModal = () => {
		showDetailsModal = !showDetailsModal;
	}

	const handleDetailsModal = (product) => {
		detailsProduct = product;
		switchDetailsModal();
	}


	const handleCreateModal = () => {		
		switchCreateModal();
	}

	const handleEditModal = (product) => {
		editedProduct = product;
		switchEditModal();
	}

	const addProduct = (e) => {
		let data = e.detail;
		fetch("https://localhost:44364/api/products/", {
			method: "POST",
			headers: {
				"Content-Type": "application/json",
			},
			body: JSON.stringify(data),
		})
		.then((response) => response.json())
		.then((data) => {
			console.log("Success:", data);
		})
		.catch((error) => {
			console.error("Error:", error);
		});
  	};

	const editProduct = (e) =>{
		let data = e.detail;
		console.log(data);
		fetch("https://localhost:44364/api/products/" + data.id, {
			method: "PUT",
			headers: {
				"Content-Type": "application/json",
			},
			body: JSON.stringify(data),
		})
		.then((data) => {
			console.log("Success:", data);
		})
		.catch((error) => {
			console.error("Error:", error);
		});
		
  	};

	const handleDelete = (id) => {		
		fetch("https://localhost:44364/api/products/" + id, {
			method: "DELETE",
			headers: {
				"Content-Type": "application/json",
			},
		})
		.catch((error) => {
			console.error("Error:", error);
		});
		products = products.filter((product) => product.id != id);
	}	

	const handleInfo = (product) => {		
		console.log(product.id);
	}

</script>

<ProductDetails product={detailsProduct} showDetailsModal={showDetailsModal} on:click={switchDetailsModal} />
<EditModal product={editedProduct} showEditModal={showEditModal} on:click={switchEditModal}>
	<slot>
		<EditProductForm product={editedProduct} on:click={switchEditModal} on:editProduct={editProduct} />
	</slot>
</EditModal>
<CreateModal showCreateModal={showCreateModal} on:click={switchCreateModal}>
	<slot>
		<AddProductForm on:click={switchCreateModal} on:addProduct={addProduct} />
	</slot>
</CreateModal>
<main>
	<section class="hero is-info is-small mb-4">
		<div class="hero-body">
		  <p class="title">
			List of Products
		  </p>
		  <button on:click={() => {handleCreateModal()}} class="button is-success is-medium mx-2">Create</button>
		</div>
	  </section>
	<div class="columns">
		<div class="is-three-quarters">
			
		</div>
	</div>
	<div class="table-container mx-4">
		<table class="table is-bordered is-striped is-fullwidth is-hoverable ">
			<thead>
				<tr>
					<th>ID</th>
					<th>Name</th>
					<th>In Stock</th>
					<th>Info</th>
					<th>Edit</th>
					<th>Delete</th>
				</tr>
			</thead>
			{#each products as product (product.id)} 
				<tbody>
					<tr>
						<th>{product.id}</th>
						<!-- svelte-ignore a11y-missing-attribute -->
						<th>
							<a on:click={() => {handleDetailsModal(product)}}>{product.name}</a>
						</th>
						{#if product.inStock == true }
							<th class = "has-text-success is-2">Available</th>
						{:else}
							<th class = "has-text-danger is-2">Unavailable</th>
						{/if}
						<th>
							<button on:click={() => {handleDetailsModal(product)}} class="button is-info mx-2">Details</button>						
						</th>
						<th>
							<button on:click={() => {handleEditModal(product)}} class="button is-warning mx-2">Edit</button>						
						</th>
						<th>
							<button on:click={() => {handleDelete(product.id)}} class="button is-danger mx-2">Delete</button>
						</th>
					</tr>
				</tbody>					
			{:else}
				<em>Loading...</em>
			{/each}
		</table>
	</div>
	
	
	
</main>
<Footer />

<style>
	main {
		text-align: center;
		max-width: 240px;
		margin: 0 auto;
		display: flex;
		min-height: 100vh;
		flex-direction: column;
	}


	@media (min-width: 340px) {
		main {
			max-width: none;
		}
	}
</style>