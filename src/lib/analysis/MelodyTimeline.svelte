<script>
	import * as d3 from "d3";
	import { onMount, afterUpdate } from "svelte";
	import Notation from "$lib/analysis/Notation.svelte";

	export let data;

	let melodyName, timestamp, originalTempo, alteredTempo, originalMelody, alteredMelody, truncation, duration, pitches;
	let melodyBars, width, height, margin, x, T, f, xAxis;

	function createAxis(svgId) {
		const svg = d3.select(`#${svgId}`);

		// Clear existing elements
		svg.selectAll("*").remove();

		// Define dimensions and scales
		width = 900;
		height = 75;
		margin = { top: 20, right: 20, bottom: 20, left: 0 };
		x = d3
			.scaleTime()
			.domain([new Date("2024-10-17T10:15:43"), new Date("2024-10-17T10:20:57")])
			.range([margin.left, width - margin.right]);
		T = x.ticks();
		f = x.tickFormat();
		xAxis = d3.axisBottom(x).tickValues(T).tickFormat(f);

		// Append the axis
		svg.append("g")
			.attr("transform", `translate(0, ${height - margin.bottom})`)
			.call(xAxis);
	}

	function createMelodyBars() {
		const melodies = data.source;
		const startMinutes = 15;
		const startSeconds = 43;
		const endMinutes = 20;
		const endSeconds = 57;
		const bars = [];

		function extractTime(string) {
			const time = string.match(/\d{2}:\d{2}:\d{2}/);
			return time ? time[0] : null;
		}

		function determineLengthOfMelody(melody) {
			const tempo = melody.alteredTempo;
			const notes = melody.alteredMelody;
			let length = 0;

			for (const note of notes) {
				switch (note[1]) {
					case "2n":
						length += 2;
						break;
					case "4n":
						length += 1;
						break;
					case "8n":
						length += 0.5;
						break;
					case "16n":
						length += 0.25;
						break;
					case "2n.":
						length += 3;
						break;
					case "4n.":
						length += 1.5;
						break;
					case "8n.":
						length += 0.75;
						break;
					case "16n.":
						length += 0.375;
						break;
					case "2n..":
						length += 3.5;
						break;
					case "4n..":
						length += 1.75;
						break;
					case "8n..":
						length += 0.875;
						break;
					case "16n..":
						length += 0.4375;
						break;
					default:
						length += 1;
						break;
				}
			}

			return (length / tempo) * 60;
		}

		function determineEndTime(melody, start, end) {
			const length = determineLengthOfMelody(melody);

			if (start[2] + length < 60) {
				end.push(start[1], start[2] + length);
			} else {
				end.push(start[1] + 1, start[2] + length - 60);
			}
		}

		for (const melody of melodies) {
			const time = extractTime(melody.timestamp)
				.split(":")
				.map((n) => Number(n));
			const bar = {};
			let startTime = [10];
			let endTime = [10];

			if (time[1] === startMinutes) {
				if (time[2] >= startSeconds) {
					startTime.push(time[1], time[2]);
					determineEndTime(melody, startTime, endTime);
				}
			}

			if (time[1] > startMinutes && time[1] < endMinutes) {
				startTime.push(time[1], time[2]);
				determineEndTime(melody, startTime, endTime);
			}

			if (time[1] === endMinutes) {
				if (time[2] <= endSeconds) {
					startTime.push(time[1], time[2]);
					determineEndTime(melody, startTime, endTime);
				}
			}

			startTime = startTime
				.map((n, i) => {
					if (i === 2 && n < 10) {
						return `0${n}`;
					} else {
						return n;
					}
				})
				.join(":");

			endTime = endTime
				.map((n, i) => {
					if (i === 2 && n < 10) {
						return `0${n}`;
					} else {
						return n;
					}
				})
				.join(":");

			bar.startTime = startTime;
			bar.endTime = endTime;

			if (startTime.length > 1 && endTime.length > 1) {
				bars.push({
					start: new Date(`2024-10-17T${startTime}`),
					end: new Date(`2024-10-17T${endTime}`),
					melody: melody,
				});
			}
		}

		melodyBars = [];
		melodyBars = bars.filter((bar) => !isNaN(bar.start) && !isNaN(bar.end));

		const svg = d3.select("#timeline");
		const barHeight = 15;

		svg.selectAll("rect")
			.data(melodyBars)
			.enter()
			.append("rect")
			.attr("x", (d) => x(d.start))
			.attr("y", (d) => barHeight + 5)
			.attr("width", (d) => x(d.end) - x(d.start))
			.attr("height", barHeight)
			.attr("fill", data.color || "steelblue")
			.attr("data-melody", (d) => JSON.stringify(d))
			.on("click", function (event, d) {
				const melody = JSON.parse(d3.select(this).attr("data-melody"));

				svg.selectAll("rect").attr("fill", data.color || "steelblue");
				d3.select(this).attr("fill", "white");

				melodyName = melody.melody.melodyName;
				timestamp = melody.melody.timestamp;
				originalTempo = melody.melody.originalTempo;
				alteredTempo = melody.melody.alteredTempo;
				originalMelody = melody.melody.originalMelody;
				alteredMelody = melody.melody.alteredMelody;
				truncation = melody.melody.truncation;
				duration = melody.melody.duration;
				pitches = melody.melody.pitches;
			});
	}

	onMount(() => {
		melodyName = data.source[0].melodyName;
		timestamp = data.source[0].timestamp;
		originalTempo = data.source[0].originalTempo;
		alteredTempo = data.source[0].alteredTempo;
		originalMelody = data.source[0].originalMelody;
		alteredMelody = data.source[0].alteredMelody;
		truncation = data.source[0].truncation;
		duration = data.source[0].duration;
		pitches = data.source[0].pitches;

		createAxis("timeline");
		createMelodyBars();
	});

	afterUpdate(() => {
		createAxis("timeline");
		createMelodyBars();
	});
</script>

<div class="container">
	<h4>Melody Timeline</h4>
	<p>This graph illustrates all melodies as they occur over the span of the performance, which took place on October 17th 2024 from 10:15:43AM until 10:20:57AM. Clicking on individual wedges will give you info about the melody that was generated at that time.</p>
	<svg id="timeline" class="mb-3" {width} {height}></svg>
	<h5>Melody Info</h5>
	{#if melodyName}
		<ul>
			<li>Original Melody - <span class="fw-bold">{melodyName}</span></li>
			<li>This melody was generated at {timestamp.substring(12)}</li>
			<li>Original Tempo - <span class="fw-bold">{originalTempo} BPM</span></li>
			<li>
				Altered Tempo - <span class="fw-bold">{alteredTempo} BPM</span>
				{#if originalTempo !== alteredTempo}
					which is a <span class="fw-bold">{originalTempo > alteredTempo ? `${originalTempo - alteredTempo} BPM decrease` : `${alteredTempo - originalTempo} BPM increase`}</span>
				{/if}
			</li>
			<li>
				{#if Object.keys(truncation).length > 0}
					This melody was <span class="fw-bold">truncated</span> to <span class="fw-bold">{truncation.alteredLength} notes</span>. That is a <span class="fw-bold">{truncation.originalLength - truncation.alteredLength} note decrease</span>.
				{:else if Object.keys(duration).length > 0}
					The <span class="fw-bold">durations were altered</span> starting at <span class="fw-bold">note {duration.firstChange + 1}</span>.
				{:else if Object.keys(pitches).length > 0}
					The melody's <span class="fw-bold">pitches were altered</span> starting at <span class="fw-bold">note {pitches.firstChange + 1}</span>.
				{:else}
					This melody <span class="fw-bold">didn't undergo any structural alterations</span>. This is exceptionally rare, with a <span class="fw-bold">likelihood of 1 in 100</span>.
				{/if}
			</li>
		</ul>
		<div style="min-height: 380px;">
			<h5>Here is the original melody</h5>
			<Notation melody="{originalMelody}" id="{'originalMelody'}" />
			{#if Object.keys(truncation).length !== 0 || Object.keys(duration).length !== 0 || Object.keys(pitches).length !== 0}
				<h5 class="mt-3">And here's the altered melody</h5>
				<Notation melody="{alteredMelody}" id="{'alteredMelody'}" />
			{/if}
		</div>
	{/if}
</div>

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
