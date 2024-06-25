<script lang="ts">
  import axios, { AxiosError } from "axios";
  import { onMount } from "svelte";
  import Spinner from "./spinner.svelte";

  let cities: any[] = [];
  let error: string | null = null;
  let successMessage: string | null = null;
  let loadingMessage: string | null = null;

  let formValues: {
    city: string;
    country: string;
  } = {
    city: "",
    country: "",
  };

  async function handleGetData() {
    loadingMessage = "Loading.....";
    try {
      const response = await axios.get(
        "https://countriesnow.space/api/v0.1/countries/population/cities"
      );
      const citie = response?.data?.data;
      cities = citie;
      loadingMessage = null;
      error = null;
    } catch (err) {
      error = "Failed to fetch data !";
      loadingMessage = null;
      console.error(err);
    }
  }

  async function handleSubmit(event: any) {
    event.preventDefault();
    loadingMessage = "Loading.....";
    error=null;

    try {
      const response = await axios.post(
        "https://countriesnow.space/api/v0.1/countries/population/cities",
        {
          country: formValues.country,
          city: formValues.city,
        }
      );
      console.log(response, 2);
      if (response.status === 200) {
        successMessage = "Data successfully Recived!";
        alert(successMessage);
        const searchResult = response?.data?.data;
        cities = [searchResult];
        loadingMessage = null;
      } else {
        console.log(response, 1);
      }
    } catch (err) {
      const errMsg: string =
        (err as AxiosError<{ msg?: string }>).response?.data?.msg ||
        (err as AxiosError)?.message;
      loadingMessage = null;
      error = errMsg;
      console.error(err);
      console.log(
        (err as AxiosError<{ msg?: string }>)?.response?.data?.msg,
        3
      );
    }
  }

  const handleRest = () => {
    formValues = {
      city: "",
      country: "",
    };
    handleGetData();
  };

  onMount(async () => {
    handleGetData();
  });
</script>

<form on:submit={handleSubmit}>
  <div class="form-group">
    <label for="Country">Country Name</label>
    <input
      type="text"
      placeholder="Country"
      bind:value={formValues.country}
      required
    />
  </div>
  <div class="form-group">
    <label for="City">City Name</label>
    <input
      type="text"
      placeholder="City"
      bind:value={formValues.city}
      required
    />
  </div>
  <button type="submit">Search</button>
  <button on:click={handleRest}>Reset</button>
</form>

{#if loadingMessage}
  <Spinner size={60} />
  <h1>Loading.....!</h1>
{:else if error}
  <h1>{error}</h1>
{:else}
  <table>
    <thead>
      <tr>
        <th>Country</th>
        <th>City</th>
        <th>Population</th>
        <th>Year</th><th>Reliabilty</th><th>Sex</th>
      </tr>
    </thead>
    <tbody>
      {#each cities as { country, city, populationCounts }}
        {#each populationCounts as { value, year, reliabilty, sex }, i}
          {#if i === 0}
            <tr>
              <td>{country}</td>
              <td>{city}</td>
              <td>{value}</td>
              <td>{year}</td><td>{reliabilty}</td><td>{sex}</td>
            </tr>
          {/if}
        {/each}
      {:else}
        <h1>Data is loading...</h1>
      {/each}
    </tbody>
  </table>
{/if}

<style>
  table {
    width: 100%;
    border-collapse: collapse;
    border-style: solid;
    border-color: black;
  }

  th,
  td {
    border: 1px solid #ddd;
    padding: 6px;
    font-size: 22px;
  }

  th {
    background-color: #91e40d;
    text-align: center;
    border-color: black;
    height: 40px;
  }

  tbody {
    min-height: 70vh;
    overflow: scroll;
  }

  tr,
  td {
    border-color: black;
    border-style: dotted;
  }
  td {
    text-align: center;
  }

  h1 {
    text-align: center;
    justify-content: center;
    display: flex;
    align-items: center;
  }
  form {
    margin-bottom: 20px;
    display: flex;
    flex-direction: row;
    align-items: center;
    column-gap: 20px;
  }

  .form-group {
    margin-bottom: 15px;
  }

  label {
    display: block;
    margin-bottom: 5px;
    font-weight: 600;
  }

  input {
    padding: 8px;
    width: 100%;
    box-sizing: border-box;
  }

  button {
    padding: 10px 5px;
    background-color: #91e40d;
    color: white;
    border: 1px solid white;
    cursor: pointer;
    height: 40px;
    width: 100px;
    margin-top: 7px;
  }

  button:hover {
    background-color: #1352c7;
  }
</style>
