<!-- src/Countries.svelte
<script>
    import { onMount } from "svelte";
    import axios from "axios";
 
    let countries = [];
    let loading = true;
    let error = null;
 
    onMount(async () => {
       try {
          const response = await axios.get("https://restcountries.com/v3.1/all");
          countries = response.data;
       } catch (err) {
          error = "Failed to fetch countries";
       } finally {
          loading = false;
       }
    });
 </script>
 <input type="text" value={name}>


 {#if loading}
    <p>Loading...</p>
 {:else if error}
    <p>{error}</p>
 {:else}
    <ul>
       {#each countries as country}
       
       {:if country.finally(name)}
        <p>{name}</p>
        {/if}
       <li>{country.name.common}</li>
       {/each}
    </ul>
 {/if}
  -->
  <script>
    import { onMount } from "svelte";
    import axios from "axios";
 
    let countries = [];
    let loading = true;
    let error = null;
    let name = ""; // Variable name para el input

    onMount(async () => {
       try {
          const response = await axios.get("https://restcountries.com/v3.1/all");
          countries = response.data;
       } catch (err) {
          error = "Failed to fetch countries";
       } finally {
          loading = false;
       }
    });

    $: filteredCountries = countries.filter(country => 
        country.name.common.toLowerCase().includes(name.toLowerCase())
    );
</script>

<input type="text" bind:value={name} placeholder="Search countries...">

{#if loading}
    <p>Loading...</p>
{:else if error}
    <p>{error}</p>
{:else}
    <ul>
       {#each filteredCountries as country}
           <li>{country.name.common}</li>
       {/each}
    </ul>
{/if}
