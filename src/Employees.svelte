<script>
import { onMount } from "svelte";
import { fly, scale, } from 'svelte/transition';
import ClockIn from './ClockIn.svelte';
import ProgressCircles from "./ProgressCircles.svelte";
import Fa from 'svelte-fa';
import { faCalendar, faList, faCalculator, faUser } from '@fortawesome/free-solid-svg-icons';
import { Motion } from 'svelte-motion'


let awards = Math.floor(Math.random() * 5);
let attendance = Math.floor(Math.random() * 12) + "/" + Math.floor(Math.random() * 29);
let leaves = Math.floor(Math.random() * 20);
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

	onMount(fetchData);

</script>
<main>
    {#each employees as employee}

		<div class="container">
			<div class="personal-row">

				<div class="personal-info-left" transition:scale>
					<img src={employee.picture.medium} alt="img" />
					<p>{employee.name.first} {employee.name.last}</p>
					<p>{employee.phone}</p>
					<div class="attendance">
						<div class="attendance-items">
							<p>Awards</p>
							<p>{awards}</p>
						</div>
						<div class="attendance-items">
							<p>Attendance</p>
							<p>{attendance}</p>
						</div>
						<div class="attendance-items">
							<p>Leaves</p>
							<p>{leaves}</p>
						</div>
					</div>
					<div class="employee-item-container">
						<Motion let:motion whileHover={{scale:1.09}} class="icon-holder">
							<div use:motion class='employee-item'>
								<h3>Schedule</h3>
								<Fa icon={faCalendar} rotate={0} />
							</div>
						</Motion>

						<Motion let:motion whileHover={{scale:1.09}} class="icon-holder">
							<div use:motion class='employee-item'>
								<h3>List</h3>
								<Fa icon={faList} rotate={0} />
							</div>
						</Motion>
						<Motion let:motion whileHover={{scale:1.09}} class="icon-holder">
							<div use:motion class='employee-item'>
								<h3>Calculator</h3>
								<Fa icon={faCalculator} rotate={0} />
							</div>
						</Motion>

					</div>
				</div>

				<div class="personal-info-right">
					<div class="personal-data-title"><Fa icon={faUser}/>Personal Details</div>
					<table>
						<tr transition:fly={{ x: 30 }}>
							<td>Employee Name: </td>
							<td>{employee.name.first} {employee.name.last}</td>
						</tr>
						<tr transition:fly={{ x: -30 }}>
							<td>Employee Email: </td>
							<td>{employee.email}</td>
						</tr>
						<tr transition:fly={{ x: 30 }}>
							<td>Employee Gender: </td>
							<td>{employee.gender}</td>
						</tr>
						<tr transition:fly={{ x: -30 }}>
							<td>Employee City: </td>
							<td>{employee.location.city}</td>
						</tr>
						<tr transition:fly={{ x: 30 }}>
							<td>Employee Country: </td>
							<td>{employee.location.country}</td>
						</tr>
						<tr transition:fly={{ x: -30 }}>
							<td>Employee Postal Code: </td>
							<td>{employee.location.postcode}</td>
						</tr>
					</table>

					<ClockIn />
					<ProgressCircles />
				</div>
	

			</div>

		</div>

	{/each}
</main>

<style>

	.container {
		display: flex;
		flex-direction: column;
		gap: 25px;
		border-radius: 6px;
		padding: 25px;
		align-items: start;
	}

	.employee-item {
    width: 200px;
    display: flex;
    flex-direction: row;
    align-items: center;
    justify-content: space-around;
    border: 2px solid;
    border-radius: 10px;
	transition: .2s;
	cursor: pointer;
	}

	.employee-item-container {
		margin-top: 25px;
		display: flex;
		flex-direction: column;
		gap: 10px;
	}

	.employee-item:hover {
    background-color: #1098f7;
    color: white;
}

    .personal-data-title {
		background-color: #1098f7;
		height: 50px;
		display: flex;
		align-items: center;
		justify-content: center;
		color: white;
		font-size: 20px;
		font-family: "Poppins", sans-serif;
		width: 100%;
		gap: 10px;
	}


	/* Personal Row Styles */
	.personal-row {
		display: flex;
		gap: 25px;
		padding: 25px;
		align-items: start;
		width: 100%;
	}

    /* Personal Info Left Styles */
	.personal-info-left {
		display: flex;
		flex-direction: column;
		align-items: center;
		width: 280px;
	}

	.personal-info-left p {
		font-size: 12px;
		margin: 10px 0 0 0;
	}

	/* Attendance Items Styles */
	.attendance-items {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	/* Table Styles */
	table {
		font-family: arial, sans-serif;
		border-collapse: collapse;
		width: 100%;
		border-radius: 6px;
	}

	td {
		border: 1px solid #e0e0e0;
		text-align: left;
		padding: 8px;
	}

	tr:nth-child(even) {
		background-color: #e0e0e0;
	}

	/* Personal Info Right Styles */
	.personal-info-right {
		padding: 25px;
		background: #f5f5f5;
		border: none;
		border-radius: 6px;
		width: 100%;
	}

	/* Attendance Styles */
	.attendance p {
		margin: 0;
	}

	.attendance {
		width: 100%;
		display: flex;
		flex-direction: row;
		justify-content: space-around;
		background-color: dodgerblue;
		height: 30px;
		align-items: center;
		margin-top: 15px;
		color: white;
		padding: 10px;
		border-radius: 6px;
	}

    /* Personal Info Left Image Styles */
	.personal-info-left img {
		width: 100px;
		box-shadow: 5.5px 11px 11px hsl(0deg 0% 0% / 0.33);
		border-radius: 6px;
	}

    	/* Media Query for Small Screens */
	@media screen and (max-width: 600px) {
		.container {
			width: 100%;
			padding: 0;
		}

		td {
			font-size: 8px;
		}

		.personal-row {
			flex-direction: column;
			align-items: center;
			width: 100%;
			padding: 25px 0;
		}

		.personal-info-left {
			width: 100%;
		}

		.personal-info-right {
			width: 100%;
			padding: 0;
		}

		.employee-item {
			width: 100%;
		}


		.employee-item-container {
			width: 100%;
		}
	}


</style>