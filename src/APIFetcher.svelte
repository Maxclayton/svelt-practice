<script>
 let employees = [];

async function fetchData() {
  try {
    const response = await fetch("https://randomuser.me/api/");
    if (response.ok) {
      const data = await response.json();
      employees = data.results;
      console.log(employees);
    } else {
      console.error("Failed to fetch data from the API");
    }
  } catch (error) {
    console.error("An error occurred while fetching data:", error);
  }
}

fetchData();

    export { employees };

  </script>