<script lang="ts">
  import { onMount } from "svelte";

  let products = [];
  onMount(async () => {
    products = await getProducts();
  });

  const getProducts = async () => {
    const res = await fetch("https://jsonplaceholder.typicode.com/photos");
    const data = await res.json();
    const filterData = data.filter((item: any) => {
      return item.id % 100 === 0;
    });
    return filterData;
  };
</script>

{#await getProducts()}
  <p>...please wait while data is fetched</p>
{:then data}
  <!-- {JSON.stringify(data)} -->
  {#each data as { id, title, url }}
    <a href="_blank">
      <div class="m-2 rounded-md h-fit bg-slate-200">
        <div
          class="rounded-md flex max-w-4 hover:shadow-sm hover:shadow-current overflow-hidden"
        >
          <img src={url} class="w-1/6 p-2 h-28 ml-2 object-cover" alt="img" />
          <div class="w-5/6 pl-4 pt-2">
            <!-- <h1 class="text-gray-700 font-semibold text-2xl">Product Title</h1> -->
            <p class="text-md">
              {title}
            </p>
            <p class="text-2xl pt-2 text-red-600">{id}</p>
          </div>
        </div>
      </div>
    </a>
  {/each}
{/await}
