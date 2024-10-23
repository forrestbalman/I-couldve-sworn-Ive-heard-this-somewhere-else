<script>
	import { onMount, afterUpdate } from "svelte";
	import { Renderer, Stave, StaveNote, Voice, Formatter, Accidental, Dot, Beam } from "vexflow";

	export let melody, id;
	let renderer, context, stave;

	function renderNotation() {
		const div = document.getElementById(id);
		div.innerHTML = "";
		renderer = new Renderer(div, Renderer.Backends.SVG);
		renderer.resize(900, 150);

		context = renderer.getContext();

		stave = new Stave(10, 20, 850);
		stave.addClef("treble").addTimeSignature("4/4");
		stave.setContext(context).draw();

		function generateNotes() {
			const notes = [];

			function formatPitch(note) {
				if (note[0].includes("rest")) {
					return "b/4";
				} else {
					return `${note[0].substring(0, note[0].length - 1).toLowerCase()}/${note[0][note[0].length - 1]}`;
				}
			}

			function formatRhythm(note) {
				let rhythm;

				switch (note[1]) {
					case "2n":
						rhythm = "h";
						break;
					case "4n":
						rhythm = "q";
						break;
					case "8n":
						rhythm = "8";
						break;
					case "16n":
						rhythm = "16";
						break;
					case "2n.":
						rhythm = "hd";
						break;
					case "4n.":
						rhythm = "qd";
						break;
					case "8n.":
						rhythm = "8d";
						break;
					case "16n.":
						rhythm = "16d";
						break;
					case "2n..":
						rhythm = "hdd";
						break;
					case "4n..":
						rhythm = "qdd";
						break;
					case "8n..":
						rhythm = "8dd";
						break;
					case "16n..":
						rhythm = "16dd";
						break;
					default:
						rhythm = "q";
						break;
				}

				if (note[0].includes("rest")) {
					rhythm += "r";
				}

				return rhythm;
			}

			function dotted(staveNote) {
				Dot.buildAndAttach([staveNote]);
				return staveNote;
			}

			for (const note of melody) {
				const pitch = formatPitch(note);
				const rhythm = formatRhythm(note);

				let n = new StaveNote({
					keys: [pitch],
					duration: rhythm,
				});

				if (n.keys[0].includes("#")) {
					n.addModifier(new Accidental("#"), 0);
				}

				if (rhythm.includes("d")) {
					n = dotted(n);
				}

				notes.push(n);
			}

			return notes; // Ensure the notes array is returned
		}

		function findNumberOfBeats() {
			let beats = 0;

			for (const note of melody) {
				switch (note[1]) {
					case "2n":
						beats += 2;
						break;
					case "4n":
						beats += 1;
						break;
					case "8n":
						beats += 0.5;
						break;
					case "16n":
						beats += 0.25;
						break;
					case "4n.":
						beats += 1.5;
						break;
					case "8n.":
						beats += 0.75;
						break;
					case "16n.":
						beats += 0.375;
						break;
					case "2n..":
						beats += 3.5;
						break;
					case "4n..":
						beats += 1.75;
						break;
					case "8n..":
						beats += 0.875;
						break;
					case "16n..":
						beats += 0.4375;
						break;
					default:
						beats += 1;
						break;
				}
			}

			return beats;
		}

		const notes = generateNotes();
		const beams = Beam.generateBeams(notes);
		Formatter.FormatAndDraw(context, stave, notes);
		beams.forEach((b) => {
			b.setContext(context).draw();
		});
	}

	onMount(() => {
		renderNotation();
	});

	afterUpdate(() => {
		renderNotation();
	});
</script>

<div {id} class="rounded-2 bg-white"></div>

<style>
	@media (max-width: 992px) {
		div {
			display: none;
		}
	}
</style>
