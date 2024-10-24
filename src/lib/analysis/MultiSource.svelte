<script>
	import { onMount } from "svelte";
	import Notation from "$lib/analysis/Notation.svelte";
	import aryaComputer from "$lib/analysis/Arya_computer.json";
	import aryaPhone from "$lib/analysis/Arya_phone.json";
	import emersonComputer from "$lib/analysis/Emerson_computer.json";
	import forrestComputer from "$lib/analysis/Forrest_computer.json";
	import forrestTablet from "$lib/analysis/Forrest_tablet.json";
	import kolawolePhone from "$lib/analysis/Kolawole_phone.json";
	import laurenComputer from "$lib/analysis/Lauren_computer.json";
	import michaelComputer from "$lib/analysis/Michael_computer.json";
	import nicolComputer from "$lib/analysis/Nicol_computer.json";
	import nicolPhone from "$lib/analysis/Nicol_phone.json";
	import siamakComputer from "$lib/analysis/Siamak_computer.json";
	import tylerComputer from "$lib/analysis/Tyler_computer.json";
	import tylerPhone from "$lib/analysis/Tyler_phone.json";
	import vidulaComputer from "$lib/analysis/Vidula_computer.json";

	let melodies = [
		{
			name: "Arya's computer",
			source: aryaComputer,
			checked: true,
		},
		{
			name: "Arya's phone",
			source: aryaPhone,
			checked: true,
		},
		{
			name: "Emerson's computer",
			source: emersonComputer,
			color: "#7434a4",
			checked: true,
		},
		{
			name: "Forrest's computer",
			source: forrestComputer,
			color: "#735c99",
			checked: true,
		},
		{
			name: "Forrest's tablet",
			source: forrestTablet,
			color: "#735c99",
			checked: true,
		},
		{
			name: "Kolawole's phone",
			source: kolawolePhone,
			checked: true,
		},
		{
			name: "Lauren's computer",
			source: laurenComputer,
			checked: true,
		},
		{
			name: "Michael's computer",
			source: michaelComputer,
			checked: true,
		},
		{
			name: "Nicol's computer",
			source: nicolComputer,
			color: "#FF69B4",
			checked: true,
		},
		{
			name: "Nicol's phone",
			source: nicolPhone,
			color: "#FF69B4",
			checked: true,
		},
		{
			name: "Siamak's computer",
			source: siamakComputer,
			checked: true,
		},
		{
			name: "Tyler's computer",
			source: tylerComputer,
			checked: true,
		},
		{
			name: "Tyler's phone",
			source: tylerPhone,
			checked: true,
		},
		{
			name: "Vidula's computer",
			source: vidulaComputer,
			checked: true,
		},
	];
	let selectedMelodyData;

	function handleCheck(melody) {
		melody = { ...melody, checked: !melody.checked };
		updateMelodies();
	}

	function updateMelodies() {
		melodies = [...melodies];
	}

	function timeArray(timestamp) {
		let time = timestamp.split(" ");

		if (time.includes("पू")) {
			return time[2].split(":").map((n) => Number(n));
		} else {
			return time[1].split(":").map((n) => Number(n));
		}
	}

	function createChartData() {
		for (const participant of melodies) {
			const melodyPlots = [];

			participant.source = participant.source.filter((melody) => checkTime(timeArray(melody.timestamp)));

			for (const melody of participant.source) {
				const timestamp = timeArray(melody.timestamp);
				const startInSeconds = melodyStartSecondsIntoPerformance(timestamp);
				const durationInSeconds = melodyDurationInSeconds(melody.alteredMelody, melody.alteredTempo);
				const x = convertStartToPercentage(startInSeconds);
				const width = convertDurationToPercentage(durationInSeconds);
				const metaData = melody;

				melodyPlots.push({
					timestamp,
					startInSeconds,
					durationInSeconds,
					x,
					width,
					metaData,
				});
			}

			function checkTime(array) {
				if (array[1] < 15 || array[1] > 20) {
					return false;
				} else {
					if (array[1] === 15 && array[2] < 43) {
						return false;
					} else if (array[1] === 20 && array[2] > 57) {
						return false;
					} else {
						return true;
					}
				}
			}

			function melodyStartSecondsIntoPerformance(array) {
				const startMinutes = 15;
				const startSeconds = 43;

				return (array[1] - startMinutes) * 60 + (array[2] - startSeconds);
			}

			function melodyDurationInSeconds(melody, tempo) {
				let duration = 0;

				for (let i = 0; i < melody.length; i++) {
					const note = melody[i][1];

					switch (note) {
						case "2n":
							duration += 2;
							break;
						case "4n":
							duration += 1;
							break;
						case "8n":
							duration += 0.5;
							break;
						case "16n":
							duration += 0.25;
							break;
						case "2n.":
							duration += 3;
							break;
						case "4n.":
							duration += 1.5;
							break;
						case "8n.":
							duration += 0.75;
							break;
						case "16n.":
							duration += 0.375;
							break;
						case "2n..":
							duration += 3.5;
							break;
						case "4n..":
							duration += 1.75;
							break;
						case "8n..":
							duration += 0.875;
							break;
						case "16n..":
							duration += 0.4375;
							break;
						default:
							break;
					}
				}

				return duration * (60 / tempo);
			}

			function convertStartToPercentage(time) {
				const performanceDuration = 314; // total performance duration in seconds
				return (time / performanceDuration) * 100;
			}

			function convertDurationToPercentage(duration) {
				const performanceDuration = 314; // total performance duration in seconds
				return (duration / performanceDuration) * 100;
			}

			participant.bars = melodyPlots;
		}

		updateMelodies();
	}

	$: console.log(selectedMelodyData);

	onMount(() => {
		createChartData();
	});
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<section id="multi-source" class="min-vh-100 container d-flex flex-column justify-content-center align-items-center pb-5">
	<div class="mb-3">
		<h1 class="text-center mb-3 pt-5">Multi Source Analysis</h1>
		<p>This section provides an analysis of multiple sources of data collected during the performance. It allows for a comparative view of the melodies generated from different devices.</p>
		<div class="row">
			{#each melodies as melody, i (melody.name)}
				<div class="col-3">
					<div class="form-check me-2">
						<input
							class="form-check-input"
							type="checkbox"
							id="{melody.name}"
							value="{melody.source}"
							bind:checked="{melody.checked}"
							on:change="{() => {
								handleCheck(melody);
							}}" />
						<label class="form-check-label user-select-none" for="{melody.name}">{melody.name}</label>
					</div>
				</div>
			{/each}
		</div>
	</div>
	<div class="mb-5 w-100">
		<div class="row w-100" style="min-height: 350px;">
			{#each melodies as melody}
				{#if melody.checked}
					<div class="col-2 p-0 d-flex align-items-center justify-content-end">
						{#if melody.name.includes("computer")}
							<p class="m-0">{melody.name.replace("computer", "💻")}</p>
						{:else if melody.name.includes("tablet")}
							<p class="m-0">{melody.name.replace("tablet", "📱")}</p>
						{:else if melody.name.includes("phone")}
							<p class="m-0">{melody.name.replace("phone", "📱")}</p>
						{/if}
					</div>
					<div class="col-10 d-flex p-0 position-relative overflow-hidden">
						{#if melody.bars}
							{#each melody.bars as bar}
								<div class="position-absolute top-50 translate-middle-y" style="left: {bar.x}%; width: {bar.width}%; height: 10px; background: {melody.color || 'steelblue'};" on:click="{() => (selectedMelodyData = bar.metaData)}"></div>
							{/each}
						{/if}
					</div>
				{/if}
			{/each}
		</div>
		<div class="row mt-3">
			<div class="col-2"></div>
			<div class="col-10 p-0 position-relative">
				<div class="timeline w-100 position-relative border-top border-white">
					{#each [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10] as i}
						<p class="time-marker position-absolute" style="left: {i * 10}%; transform: translateX(-50%);">
							{#if i === 0}
								10:15:43
							{:else if i === 1}
								10:16:13
							{:else if i === 2}
								10:16:43
							{:else if i === 3}
								10:17:13
							{:else if i === 4}
								10:17:43
							{:else if i === 5}
								10:18:13
							{:else if i === 6}
								10:18:43
							{:else if i === 7}
								10:19:13
							{:else if i === 8}
								10:19:43
							{:else if i === 9}
								10:20:13
							{:else if i === 10}
								10:20:57
							{/if}
						</p>
					{/each}
				</div>
			</div>
		</div>
	</div>
	<div class="w-100">
		<h4>Selected Melody Data</h4>
		{#if selectedMelodyData}
			<ul>
				<li>Original Melody - <span class="fw-bold">{selectedMelodyData.melodyName}</span></li>
				<li>This melody was generated at - <span class="fw-bold">{timeArray(selectedMelodyData.timestamp).join(":")} AM</span></li>
				<li>Original Tempo - <span class="fw-bold">{selectedMelodyData.originalTempo} BPM</span></li>
				<li>
					{#if selectedMelodyData.truncation.truncated}
						This melody was <span class="fw-bold">truncated</span>, reducing its length from <span class="fw-bold">{selectedMelodyData.truncation.originalLength} to {selectedMelodyData.truncation.alteredLength}</span> notes.
					{:else if selectedMelodyData.duration.altered}
						This melody had its <span class="fw-bold">rhythms altered</span> starting at note <span class="fw-bold">{selectedMelodyData.duration.firstChange}</span>.
					{:else if selectedMelodyData.pitches.altered}
						This melody had its <span class="fw-bold">pitches altered</span>, starting at note <span class="fw-bold">{selectedMelodyData.pitches.firstChange}</span>.
					{:else}
						This melody was <span class="fw-bold">not altered</span>. There is a 1% chance for a melody to occur without any structural alterations.
					{/if}
				</li>
			</ul>
			<h5>Here's the original melody</h5>
			<Notation melody="{selectedMelodyData.originalMelody}" id="multisource-original" />
			{#if selectedMelodyData.truncation.truncated || selectedMelodyData.duration.altered || selectedMelodyData.pitches.altered}
				<h5 class="mt-3">Here's the altered melody</h5>
				<Notation melody="{selectedMelodyData.alteredMelody}" id="multisource-altered" />
			{/if}
		{:else}
			<p>Click on a melody to see more information</p>
		{/if}
	</div>
</section>

<style>
	.container {
		max-width: 900px;
	}

	@media (max-width: 992px) {
		.container {
			display: none;
		}
	}
</style>
