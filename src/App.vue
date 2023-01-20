<template>
    <div class="container">
        <form @submit="onSubmit" class="search-form">
            <div class="form-control">
                <label>Запрос </label>
                <input type="text" v-model="request" name="request" placeholder="Введите запрос" />
            </div>
            <div class="form-control">
                <label>Мой бренд </label>
                <input type="text" v-model="brand" name="brand" placeholder="Введите бренд" />
            </div>
            <input type="submit" value="Поиск" class="btn">
        </form>

        <ol>
            <li v-for="product in products" :key="product" :class="{ mybrand: isMyBrand(product.brand) }">
                {{ product.brand }}
            </li>
        </ol>
    </div>
</template>

<script>

export default {
    name: 'App',
    data() {
        return {
            request: '',
            brand: '',
            products: null
        }
    },
    methods: {
        async onSubmit(e) {
            e.preventDefault()
            if (!this.request || !this.brand) {
                alert('Пожалуйста, введите ваш запрос и бренд!')
                return
            }

            const querryParams = {
                appType: 1,
                couponsGeo: [12, 3, 18, 15, 21],
                curr: 'rub',
                dest: [-1029256, - 102269, -2162196, -1257786],
                emp: 0,
                lang: 'ru',
                locale: 'ru',
                pricemarginCoeff: 1.0,
                query: this.request,
                reg: 0,
                regions: [80, 64, 83, 4, 38, 33, 70, 68, 69, 86, 75, 30, 40, 48, 1, 66, 31, 22, 71],
                resultset: 'catalog',
                sort: 'popular',
                spp: 0,
                suppressSpellcheck: false,
            }
            const response = await fetch('https://search.wb.ru/exactmatch/ru/common/v4/search?' + new URLSearchParams(querryParams))
            const data = await response.json()
            console.log(data);
            this.products = data.data.products
        },
        isMyBrand(brand) {
            return brand.toLowerCase() === this.brand.toLowerCase()
        }
    }
}
</script>
    

<style>
.container {
    width: 50vw;
    margin: auto;
}

label {
    display: inline-block;
    width: 100px;
}

.mybrand {
    font-size: 20px;
    color: red;
}
</style>
