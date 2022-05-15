<script setup>
const contents = useState('contents', () => null);
async function fetchData() {
    try {
        const { data } = await useFetch(`https://api.nomics.com/v1/currencies/ticker?key=1da999db0f7e0fa4f70d97f48d6ffe612260cf18&interval=1d,30d&convert=TRY`); // prettier-ignore
        if (!data) throw new Error('cannot fetch data');
        contents.value = data;
        console.log(data.value)
    } catch (error) {
        console.error(error.message);
        contents.value = null;
    }
}
</script>

<template>
    <div class="space-y-4">
        <div>
            <label class="block font-bold">Choose Character ID</label>
            <div class="mt-2 space-x-4">
                <button @click="fetchData" class="py-2 px-4 bg-green-500 hover:bg-green-600 text-white rounded">Fetch
                </button>
            </div>
        </div>

        <template v-if="contents">
            <!-- <div v-for="content in contents" >
              <span>{{ content.id }}</span>
            </div> -->
            <table class="border-collapse border border-slate-400 table-auto" v-for="content in contents">
                <thead>
                    <tr >
                        <th>{{content.id}}</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td>{{content.price}}</td>
                        
                    </tr>
                    <tr>
                        <td>{{content.rank}}</td>
                        
                    </tr>
                    <tr>
                        <td>{{content.symbol}}</td>
                        
                    </tr>
                </tbody>
            </table>
        </template>
    </div>
</template>