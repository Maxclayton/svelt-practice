<script>
	import { onMount } from "svelte";
	import CircleProgressBar from "./CircleProgressBar.svelte";

	let employees = [];
	let intervalId;
	let isClockRunning = true;
	let awards = Math.floor(Math.random() * 5);
	let attendance = Math.floor(Math.random() * 12) + "/" + Math.floor(Math.random() * 29);
	let leaves = Math.floor(Math.random() * 20);
	import {
    fly,
    scale, } from 'svelte/transition';

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

	function getRandomInt(min, max) {
		return Math.floor(Math.random() * (max - min + 1)) + min;
	}

	let hours = getRandomInt(0, 7.99);
	let minutes = getRandomInt(0, 59);
	let seconds = getRandomInt(0, 59);
	let time = 0;

	function formatTime(hours, minutes, seconds) {
		const formattedMinutes = minutes < 10 ? `0${minutes}` : minutes;
		return `${hours}:${formattedMinutes}:${seconds}`;
	}

	function updatePercentage() {
		const totalSeconds = hours * 3600 + minutes * 60 + seconds;
		const percentage = (totalSeconds / (8 * 3600)) * 100;
		time = formatTime(hours, minutes, seconds);
		// timePercent = (percentage * 0.001).toFixed(2);
	}

	function myFunction() {
		if (isClockRunning) {
			clearInterval(intervalId);
			isClockRunning = false;
			document.getElementById("clock-message").innerHTML =
				"Thanks for clocking out!";
			document.getElementById("btn").innerHTML = "Clock In";
			setTimeout(() => {
				document.getElementById("clock-message").innerHTML = "";
			}, 5000);
		} else {
			document.getElementById("clock-message").innerHTML =
				"Thanks for clocking in!";
			document.getElementById("btn").innerHTML = "Clock Out";
			setTimeout(() => {
				document.getElementById("clock-message").innerHTML = "";
			}, 5000);
			intervalId = setInterval(() => {
				seconds++;
				if (seconds >= 60) {
					seconds = 0;
					minutes++;
					if (minutes >= 60) {
						minutes = 0;
						hours++;
						if (hours >= 8) {
							hours = 8;
						}
					}
				}
				updatePercentage();
			}, 1000);
			isClockRunning = true;
		}
	}

	intervalId = setInterval(() => {
		seconds++;
		if (seconds >= 60) {
			seconds = 0;
			minutes++;
			if (minutes >= 60) {
				minutes = 0;
				hours++;
				if (hours >= 8) {
					hours = 8;
				}
			}
		}
		updatePercentage();
	}, 1000);
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
			</div>

			<div class="personal-info-right">
				<div class="personal-data-title">Personal Details</div>
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

				<div class="clock-in-container">
					<div class="personal-data-title">Time In</div>
					<div class="circle" transition:scale>
						Clocked in for
						<span class="time">{time}</span>
						<button id="btn" on:click={myFunction}>
							Clock Out
						</button>
						<div id="clock-message" />
					</div>
				</div>
			</div>
		</div>
		<div class="graph-container">
			<div transition:fly={{ delay: 250, duration: 300, x: 100, y: 500, opacity: 0.5 }}>
				<CircleProgressBar progress={Math.random()} />
			</div>
			<div transition:fly={{ delay: 350, duration: 300, x: 100, y: 500, opacity: 0.5 }}>
				<CircleProgressBar progress={Math.random()} />
			</div>
			<div transition:fly={{ delay: 450, duration: 300, x: 100, y: 300, opacity: 0.5 }}>
				<CircleProgressBar progress={Math.random()} />
			</div>
		</div>
	</div>
	{/each}
</main>

<style>
	/* Fonts */
	@import url("https://fonts.googleapis.com/css2?family=Inter:wght@200;400;800&family=Poppins:ital,wght@0,300;0,400;1,400&display=swap");

	/* Container Styles */
	.container {
		display: flex;
		flex-direction: column;
		gap: 25px;
		border: 2px solid silver;
		border-radius: 6px;
		padding: 25px;
		align-items: start;
	}

	/* Personal Row Styles */
	.personal-row {
		display: flex;
		gap: 25px;
		padding: 25px;
		align-items: start;
		width: 100%;
	}

	/* Personal Data Title Styles */
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

	td,
	th {
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

	/* Clock In Container Styles */
	.clock-in-container {
		display: flex;
		flex-direction: column;
		align-items: center;
	}

	/* Circle Styles */
	.circle {
		height: 300px;
		display: flex;
		align-items: center;
		flex-direction: column;
		justify-content: center;
	}

	.circle span {
		font-size: 75px;
	}

	/* Personal Info Left Image Styles */
	.personal-info-left img {
		width: 100px;
		box-shadow: 5.5px 11px 11px hsl(0deg 0% 0% / 0.33);
		border-radius: 6px;
	}

	/* Button Styles */
	button {
		color: #333;
		background-color: dodgerblue;
		outline: none;
		width: 200px;
		color: white;
		border-radius: 6px;
		height: 40px;
		cursor: pointer;
		transition: 0.2s;
		border: none;
	}

	button:hover {
		background-color: #4db2f9;
	}

	/* Graph Container Styles */
	.graph-container {
		display: flex;
		justify-content: space-around;
		width: 100%;
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
			width: auto;
		}

		.personal-info-right {
			width: 100%;
			padding: 0;
		}
	}
</style>
