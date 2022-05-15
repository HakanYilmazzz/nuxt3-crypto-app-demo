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
    <div
        class="space-y-4 bg-gradient-to-r from-violet-500 to-fuchsia-500bg-gradient-to-r from-violet-500 to-fuchsia-500">

        <div class="space-x-4 pt-4 flex justify-center">
            <button @click="fetchData" class="py-2 px-4 bg-indigo-500 hover:bg-indigo-600 text-white rounded">Fetch Data
            </button>
        </div>

        <template v-if="contents">
            <div class="flex justify-center">
                <table class="w-3/6 text-center table table-zebra rounded-lg"
                    style="border-collapse:separate; border-spacing:0px 20px">
                    <thead class="bg-gray-900 rounded-lg shadow-lg shadow-gray-500">
                        <tr class="w-full text-white ">
                            <th class="uppercase px-6 py-4">Logo</th>
                            <th class="uppercase px-6 py-4">Name</th>
                            <th class="uppercase px-6 py-4">Price</th>
                        </tr>
                    </thead>
                    <tbody class="p-2 border-solid border-1  bg-white  text-center ">
                        <tr v-for="(content, index) in contents" class="shadow-lg shadow-cyan-500/50">
                            <td class=" rounded-l-lg"><img :src="content.logo_url" alt="" width="70"
                                    style="margin-left:25%"></td>
                            <td style="color:purple !important">{{ content.name }}<br><span
                                    class="text-white text-sm w-1/3 pb-1 bg-green-600 font-semibold px-2 rounded-full">{{
                                            content.symbol
                                    }}</span>
                            </td>
                            <td class="rounded-r-lg">{{ parseFloat(content.price).toFixed(3) }} TL</td>
                        </tr>
                    </tbody>
                </table>
            </div>
        </template>
    </div>
</template>