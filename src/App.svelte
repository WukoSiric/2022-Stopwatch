<script>
  // Times
  let totalSeconds = 0;
  let hours = 0;
  let minutes = 0;
  let seconds = 0;

  $: unitsOfTime = [hours, minutes, seconds];

  // Pause Functionality
  let startTime = Date.now();
  let stopwatchState;
  let savedSeconds = 0;
  
  function startStopwatch() {
      startTime = Date.now();
      stopwatchState = setInterval(incrementTimer, 5);
  }

  function incrementTimer() {
      let currentTime = Date.now() - startTime;
      totalSeconds = Math.floor(currentTime / 1000) + savedSeconds;
      seconds = Math.floor(totalSeconds % 60);
      minutes = Math.floor(totalSeconds / 60) % 60;
      hours = Math.floor(totalSeconds / 60 / 60) % 60;
  }

  function stopStopwatch() {
      clearInterval(stopwatchState);
      savedSeconds = totalSeconds;
  }

  function resetTimer() {
      totalSeconds = 0; 
      savedSeconds = 0;
      seconds = (totalSeconds % 60);
      stopStopwatch();
  }

  const isOneDigit = (number) => number.toString().length === 1 
</script>

<div class="wrapper">
  <h1>Stopwatch</h1>
  <div class="stopwatch">
      {#each unitsOfTime as time, i}
          {isOneDigit(time) ? "0" : "" }{time.toString() + (i < 2 ? " : " : "")}
      {/each}
  </div>
  <br />
  <div class="buttons">
      <button class="start" on:click={startStopwatch}>Start</button>
      <button class="reset" on:click={resetTimer}>reset</button>
      <button class="stop" on:click={stopStopwatch}>Stop</button>
  </div>
</div>

<style>
  .wrapper {
      margin-bottom: 50px;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      
  }

  h1 {
      color: white;
      text-decoration: underline;
  }

  .stopwatch {
      background-color: black;
      color: white;
      font-size: 5em;
      font-weight: bold;
      padding: 60px;
      border: black 10px solid;
      border-radius: 50px;
      display: flex;
      align-items: center;
  }

  .buttons {
      display: flex;
      gap: 80px;
  }

  button {
      font-size: 1.5em;
      background-color: red;
      color: white;
      font-weight: 700;
  }

  .start {
      background-color: #e1ad01;
  }

  .reset {
      background-color: black;
  }
</style>
