//const product = ref("Socks")
    //const products = ref([socks])
    //const brand = ref("Vue Mastery")
    //const image = ref(socksGreenImage)
    //const inStock = ref(false)
    //const details = ref(['50% cotton', '30% wool', '20% polyester'])
    //const variants = ref([
    //{ id: 2234, color: 'green', image: socksGreenImage },
    //{ id: 2235, color: 'blue', image: socksBlueImage },
    //])


    <div v-for="variant in product.variants" :key = "variant.id" 
        @mouseover="updateImage(variant.image)"
        class="color-circle"
        :style="{ backgroundColor: variant.color }"></div>

    <!-- Filter by brand === {{ pickedBrand }}
    <div>
    <input type="radio" id = "vueMastery" value="Vue Mastery" v-model="pickedBrand" />
    <label for="vueMastery">Vue Mastery</label>
    </div>
    <div>
    <input type="radio" id = "nike" value="Nike" v-model="pickedBrand" />
    <label for="nike">Nike</label>
    </div>
    <button class="button" v-on:click="filterByBrand({pickedBrand})">Filter</button> -->
    <div>Picked: {{ picked }}</div>

	<input type="radio" id="one" value="One" v-model="picked" />
	<label for="one">One</label>

	<input type="radio" id="two" value="Two" v-model="picked" />
	<label for="two">Two</label>



    
    const addStockFilter = () => {
        const newFilters = [...filters.value]
        if (!filters.value.includes("inStock")){
            newFilters.push("inStock")
        } else {
            newFilters.splice(newFilters.indexOf("inStock"), 1)
        }
        filters.value = newFilters
        applyFilter()
    }

    const addNikeFilter = () => {
        const newFilters = [...filters.value]
        if (!filters.value.includes("Nike")){
            newFilters.push("Nike")
        } else {
            newFilters.splice(newFilters.indexOf("Nike"), 1)
        }
        filters.value = newFilters
        applyFilter()
    }

    const addVueMasteryFilter = () => {
        const newFilters = [...filters.value]
        if (!filters.value.includes("Vue Mastery")){
            newFilters.push("Vue Mastery")
        } else {
            newFilters.splice(newFilters.indexOf("Vue Mastery"), 1)
        }
        filters.value = newFilters
        applyFilter()
    }




    Hi David,

Here is my Vue PLP. I will be sending a follow-up email shortly detailing my thought process as well as what I would have done with more time.

Kind regards,
Erik