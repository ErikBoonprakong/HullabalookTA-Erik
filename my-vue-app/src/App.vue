<script setup>
    import { ref } from 'vue'
    import { reactive } from 'vue'
    import Dropdown from 'vue-simple-search-dropdown';
    import shoeData from './data/products.json'

    const originalProducts = ref([...shoeData])
    const products = ref([...shoeData])

    const filters = ref([])
    const sort = ref("None")

    // Update the list of currently toggled filters
    const addFilter = (filter) => {
        console.log(filter)
        const newFilters = [...filters.value]
        if (!filters.value.includes(filter)){
            newFilters.push(filter)
        } else {
            newFilters.splice(newFilters.indexOf(filter), 1)
        }
        filters.value = newFilters
        applyFilter()
    }

    // Apply the filters
    const applyFilter = () => {
        // keep track of products filtered by availability and by brand separately to avoid errors when un-toggling
        let stockFilteredProductList = []
        let brandFilteredProductList = []
        if (filters.value.includes("isAvailable")){
            originalProducts.value.forEach(product => {
                if (product.isAvailable){
                    stockFilteredProductList.push(product)
                }
            })
        } else {
            stockFilteredProductList = [...originalProducts.value]
        }
        // Remove listing if there is a different brand filter
        // check for brand filters - otherwise list all brands
        if (filters.value.length < 1 || (filters.value.length === 1 && filters.value.includes("isAvailable"))){
            brandFilteredProductList = stockFilteredProductList
        } else {
            stockFilteredProductList.forEach(product => {
            if (filters.value.includes(product.brand)){
                brandFilteredProductList.push(product)
            }
            })
        }
        products.value = brandFilteredProductList
        // re-call the sort function to keep previously selected sorting options
        sortByPrice()
    }

    // Define which way the price should be sorted
    const addPriceSort = (direction) => {
        console.log("event =====")
        console.log(direction)
        sort.value = direction
        sortByPrice()
    }

    // Apply the selected price sorting criteria
    const sortByPrice = () => {
        let sortedProducts = products.value
        console.log(sort.value)
        if (sort.value === "asc"){
            console.log('asc')
            sortedProducts = sortedProducts.sort(function(a, b){return a.price-b.price})
        } else if (sort.value === "desc"){
            console.log('desc')
            sortedProducts = sortedProducts.sort(function(a, b){return b.price-a.price})
        }
        products.value = sortedProducts
    }

    // Sort by relevance function
    const sortByRelevance = () => {
        const listedProducts = products.value 
        const availables = []
        const unavailables = []
        listedProducts.forEach(product => {
            if (product.isAvailable){
                availables.push(product)
            } else {
                unavailables.push(product)
            }
        })
        availables.sort(function(a, b){return b.rank-a.rank})
        unavailables.sort(function(a, b){return b.rank-a.rank})
        products.value = [...availables, ...unavailables]
    }

</script>

<template>
  <div class="nav-bar">PRODUCT LIST</div>
  <div class="cart-results">
    <div class="cart">Results: ({{ products.length }})</div>
  </div>
  <div class="filters">
    <div class="stock-filter">
    <input type="checkbox" id="stockFilter" name="stockFilter" v-on:click="addFilter('isAvailable',$event)" />
    <label for="stockFilter">Show only in stock products</label>
    </div>
    <div class="brand-filters">
        <h3>Filter by brand:</h3>
        <input type="checkbox" id="nikeFilter" name="nikeFilter" v-on:click="addFilter('Nike',$event)" />
        <label for="nikeFilter">Nike</label>
        <input type="checkbox" id="adidasFilter" name="adidasFilter" v-on:click="addFilter('Adidas',$event)" />
        <label for="adidasFilter">Adidas</label>
        <input type="checkbox" id="newBalanceFilter" name="newBalanceFilter" v-on:click="addFilter('New Balance',$event)" />
        <label for="adidasFilter">New Balance</label>
        <input type="checkbox" id="converseFilter" name="converseFilter" v-on:click="addFilter('Converse',$event)" />
        <label for="converseFilter">Converse</label>
        <input type="checkbox" id="crocsFilter" name="crocsFilter" v-on:click="addFilter('Crocs',$event)" />
        <label for="crocsFilter">Crocs</label>
    </div>
    <div class="sort-filters">
        <h3>Sort results:</h3>
        <label for="priceSort">Price: </label>
        <select name="priceSort" id="priceSort" v-model="priceSortSelected" v-on:change="addPriceSort(priceSortSelected,$event)">
            <option value="none"> </option>
            <option value="asc">Ascending</option>
            <option value="desc">Descending</option>
        </select>
    </div>
    <div class="sort-filters">
        <button class="button" v-on:click="sortByRelevance">Sort by relevance</button>
    </div>
  </div>


  <div class="product-display">

    <table>
        
    <tr class="product-container" v-for="product in products">
        
      <td class="product-image">
        <img class="product-image" v-bind:src="product.image" />
      </td>

      <td class="product-info">
        <h1>{{ product.brand + ' ' + product.name }}</h1>
        <p>${{ product.price }}</p>
        <p>Rating: {{ product.rank }}</p>
        <p v-if="product.isAvailable">In Stock</p>
        <p v-else>Out of stock </p>

      </td>

    </tr>
    </table>
    

  </div>
</template>