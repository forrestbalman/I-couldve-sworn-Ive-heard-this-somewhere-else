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
				const border = returnBorderColor(melody);

				melodyPlots.push({
					timestamp,
					startInSeconds,
					durationInSeconds,
					x,
					width,
					border,
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

			function returnBorderColor(melody) {
				if (melody.melodyName === "Stairway to Heaven") {
					return "#fe0000";
				} else if (melody.melodyName === "Billie Jean") {
					return "#800001";
				} else if (melody.melodyName === "What a Wonderful World") {
					return "#fe6a00";
				} else if (melody.melodyName === "Sweet Child O' Mine") {
					return "#803400";
				} else if (melody.melodyName === "Hey Jude") {
					return "#ffd800";
				} else if (melody.melodyName === "Never Gonna Give You Up") {
					return "#806b00";
				} else if (melody.melodyName === "Livin' on a Prayer") {
					return "#00fe21";
				} else if (melody.melodyName === "Dancing Queen") {
					return "#007f0e";
				} else if (melody.melodyName === "Don't Stop Believin'") {
					return "#FFFFFF";
				} else if (melody.melodyName === "The Final Countdown") {
					return "#00497e";
				} else if (melody.melodyName === "Y.M.C.A") {
					return "#0026ff";
				} else if (melody.melodyName === "Sweet Home Alabama") {
					return "#001280";
				} else if (melody.melodyName === "U Can't Touch This") {
					return "#b100fe";
				} else if (melody.melodyName === "Careless Whisper") {
					return "#590080";
				}
			}

			participant.bars = melodyPlots;
		}

		updateMelodies();
	}

	onMount(() => {
		createChartData();
	});
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-static-element-interactions -->
<section id="multi-source" class="min-vh-100 container d-flex flex-column justify-content-center align-items-center py-5">
	<div class="mb-3">
		<h1 class="text-center mb-3 pt-5">Multi Source Analysis</h1>
		<p>This section provides an analysis of multiple sources of data collected during the performance. It allows for a comparative view of the melodies generated from different devices.</p>
		<div class="row mb-3">
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
		<div>
			<p>Each element's border corresponds with the source melody. The colors are as follows:</p>
			<div class="d-flex justify-content-center flex-wrap gap-2 user-select-none">
				<span class="p-1 rounded-2" style="background: #fe0000;">Stairway to Heaven</span>
				<span class="p-1 rounded-2" style="background: #800001;">Billie Jean</span>
				<span class="p-1 rounded-2" style="background: #fe6a00;">What a Wonderful World</span>
				<span class="p-1 rounded-2" style="background: #803400;">Sweet Child O' Mine</span>
				<span class="p-1 rounded-2 text-black" style="background: #ffd800;">Hey Jude</span>
				<span class="p-1 rounded-2" style="background: #806b00;">Never Gonna Give You Up</span>
				<span class="p-1 rounded-2 text-black" style="background: #00fe21;">Livin' on a Prayer</span>
				<span class="p-1 rounded-2" style="background: #007f0e;">Dancing Queen</span>
				<span class="p-1 rounded-2 text-black" style="background: #FFFFFF;">Don't Stop Believin'</span>
				<span class="p-1 rounded-2" style="background: #00497e;">The Final Countdown</span>
				<span class="p-1 rounded-2" style="background: #0026ff;">Y.M.C.A</span>
				<span class="p-1 rounded-2" style="background: #001280;">Sweet Home Alabama</span>
				<span class="p-1 rounded-2" style="background: #b100fe;">U Can't Touch This</span>
				<span class="p-1 rounded-2" style="background: #590080;">Careless Whisper</span>
			</div>
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
								<div class="position-absolute top-50 translate-middle-y rounded-1" style="left: {bar.x}%; width: {bar.width}%; height: 10px; background: {melody.color || 'steelblue'}; border: 1px solid {bar.border};" on:click="{() => (selectedMelodyData = bar.metaData)}"></div>
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

	a {
		color: rgb(145, 248, 214);
	}

	.fw-bold {
		color: rgb(248, 203, 145);
	}

	@media (max-width: 992px) {
		.container {
			display: none;
		}
	}
</style>
