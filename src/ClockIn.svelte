<script>
    import { scale } from "svelte/transition";
    import './main.css';
    import { onMount } from 'svelte';

    let showBars = false; // Flag to control when to show the bars

    let intervalId;
    let isClockRunning = true;

    onMount(() => {
    // After a delay, set the showBars flag to true to trigger the animation
    setTimeout(() => {
        showBars = true;
    }, 100);
});

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
    {#if showBars}

    <div class="clock-in-container">
        <div class="personal-data-title">Time In</div>
        <div class="circle" transition:scale>
            Clocked in for
            <span class="time">{time}</span>
            <button id="btn" on:click={myFunction}> Clock Out </button>
            <div id="clock-message" />
        </div>
    </div>
    {/if}

</main>

<style>

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
</style>
