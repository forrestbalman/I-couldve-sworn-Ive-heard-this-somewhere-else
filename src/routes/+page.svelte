<script>
	import * as Tone from "tone";
	import { onMount } from "svelte";

	let audioStarted = false;
	let analysis = [];
	let bufferCounter = 0;
	let opacities = {
		title: 0,
		play: 0,
		time: 0,
		noteSlivers: 0,
		finished: 0,
		homepage: 0,
	};
	let background = "#212529";
	let backgroundPalette = ["#212529", "#541c1c", "#54381c", "#43541c", "#1c5424", "#1c544a", "#1c4154", "#1c1d54", "#331c54", "#541c46"];
	let noteSlivers = [0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0];
	let playing = false;
	let finished = false;
	let timer = 0;
	let synths, drones, incidentals, reverb, globalVolume, username;

	const melodies = [
		{
			name: "Stairway to Heaven",
			tempo: 50,
			artist: "Led Zeppelin",
			notes: [
				["A3", "8n"],
				["C4", "8n"],
				["E4", "8n"],
				["A4", "8n"],
				["B4", "8n"],
				["E4", "8n"],
				["C4", "8n"],
				["B4", "8n"],
				["C5", "8n"],
				["E4", "8n"],
				["C4", "8n"],
				["C5", "8n"],
				["F#4", "8n"],
				["D4", "8n"],
				["A3", "8n"],
				["F#4", "8n"],
				["E4", "8n"],
				["C4", "8n"],
				["A3", "8n"],
				["C4", "4n"],
				["E4", "8n"],
				["C4", "8n"],
				["A3", "8n"],
				["G3", "8n"],
				["A3", "8n"],
				["A3", "4n"],
			],
		},
		{
			name: "Billie Jean",
			tempo: 80,
			artist: "Michael Jackson",
			notes: [
				["C#5", "8n"],
				["C#5", "8n"],
				["C#5", "8n"],
				["C#5", "8n"],
				["B4", "8n"],
				["A4", "8n"],
				["B4", "8n"],
				["A4", "8n"],
				["C#5", "4n"],
				["B4", "16n"],
				["A4", "16n"],
				["B4", "8n"],
				["A4", "8n"],
				["C#5", "4n"],
			],
		},
		{
			name: "What a Wonderful World",
			tempo: 60,
			artist: "Louis Armstrong",
			notes: [
				["C4", "8n"],
				["E4", "8n"],
				["F4", "4n."],
				["A4", "8n"],
				["C5", "2n"],
				["rest", "8n"],
				["D5", "8n"],
				["D5", "8n"],
				["D5", "8n"],
				["C5", "2n"],
				["rest", "8n"],
				["Bb4", "8n"],
				["Bb4", "8n"],
				["Bb4", "8n"],
				["A4", "2n"],
				["rest", "8n"],
				["G4", "8n"],
				["G4", "8n"],
				["G4", "8n"],
				["F4", "2n"],
			],
		},
		{
			name: "Sweet Child O' Mine",
			tempo: 100,
			artist: "Guns N' Roses",
			notes: [
				["C#4", "8n"],
				["C#5", "8n"],
				["G#4", "8n"],
				["F#4", "8n"],
				["F#5", "8n"],
				["G#4", "8n"],
				["E#5", "8n"],
				["G#4", "8n"],
				["D#4", "8n"],
				["C#5", "8n"],
				["G#4", "8n"],
				["F#4", "8n"],
				["F#5", "8n"],
				["G#4", "8n"],
				["E#5", "8n"],
				["G#4", "8n"],
			],
		},
		{
			name: "Hey Jude",
			tempo: 70,
			artist: "The Beatles",
			notes: [
				["C5", "4n"],
				["A4", "2n"],
				["rest", "8n"],
				["A4", "8n"],
				["C5", "8n"],
				["D5", "8n"],
				["G4", "2n"],
				["rest", "4n"],
				["G4", "8n"],
				["A4", "8n"],
				["A#4", "4n"],
				["F5", "4n."],
				["F5", "8n"],
				["E5", "8n"],
				["C5", "8n"],
				["D5", "8n"],
				["C5", "16n"],
				["A#4", "16n"],
				["A4", "2n"],
			],
		},
		{
			name: "Never Gonna Give You Up",
			tempo: 100,
			artist: "Rick Astley",
			notes: [
				["C5", "16n"],
				["D5", "16n"],
				["F5", "16n"],
				["D5", "16n"],
				["A5", "8n."],
				["A5", "8n."],
				["G5", "4n."],
				["C5", "16n"],
				["D5", "16n"],
				["F5", "16n"],
				["D5", "16n"],
				["G5", "8n."],
				["G5", "8n."],
				["F5", "8n."],
				["E5", "16n"],
				["D5", "8n"],
			],
		},
		{
			name: "Livin' on a Prayer",
			tempo: 120,
			artist: "Bon Jovi",
			notes: [
				["G4", "2n.."],
				["G4", "8n"],
				["G4", "4n"],
				["F#4", "4n"],
				["E4", "4n"],
				["D4", "4n"],
				["B4", "4n."],
				["C5", "4n."],
				["rest", "8n"],
				["C5", "4n"],
				["C5", "8n"],
				["B4", "8n"],
				["G4", "8n"],
				["G4", "4n"],
				["A4", "4n"],
			],
		},
		{
			name: "Dancing Queen",
			tempo: 90,
			artist: "ABBA",
			notes: [
				["C#5", "4n"],
				["B4", "8n"],
				["B4", "4n."],
				["rest", "4n"],
				["C#5", "4n"],
				["B4", "8n"],
				["B4", "4n"],
				["C#5", "4n."],
				["A4", "8n."],
				["B4", "8n."],
				["G#4", "8n"],
				["A4", "8n."],
				["B4", "8n."],
				["G#4", "8n"],
				["A4", "16n"],
				["G#4", "16n"],
				["F#4", "4n."],
			],
		},
		{
			name: "Don't Stop Believin'",
			tempo: 110,
			artist: "Journey",
			notes: [
				["A4", "4n."],
				["G#4", "4n."],
				["rest", "8n"],
				["F#4", "8n"],
				["A4", "4n."],
				["G#4", "4n."],
				["rest", "4n"],
				["rest", "2n"],
				["A4", "8n"],
				["G#4", "8n"],
				["A4", "8n"],
				["B4", "8n"],
				["C#5", "4n"],
				["B4", "16n"],
				["C#5", "16n"],
				["G#4", "4n"],
				["F#4", "4n"],
				["E4", "8n"],
			],
		},
		{
			name: "The Final Countdown",
			tempo: 104,
			artist: "Europe",
			notes: [
				["C#5", "16n"],
				["B4", "16n"],
				["C#5", "4n"],
				["F#4", "2n"],
				["rest", "8n"],
				["D5", "16n"],
				["C#5", "16n"],
				["D5", "8n"],
				["C#5", "8n"],
				["B4", "2n"],
				["rest", "8n"],
				["D5", "16n"],
				["C#5", "16n"],
				["D5", "4n"],
				["F#4", "4n"],
				["G#4", "4n"],
				["rest", "8n"],
				["B4", "16n"],
				["A4", "16n"],
				["B4", "8n"],
				["A4", "8n"],
				["G#4", "8n"],
				["B4", "8n"],
				["A4", "8n"],
			],
		},
		{
			name: "Y.M.C.A.",
			tempo: 100,
			artist: "Village People",
			notes: [
				["D4", "2n"],
				["C4", "4n"],
				["D4", "8n"],
				["C4", "8n"],
				["rest", "8n"],
				["A3", "8n"],
				["A3", "8n"],
				["G3", "8n"],
				["G3", "8n"],
				["F3", "8n"],
				["F3", "4n"],
				["G3", "2n"],
				["F3", "4n"],
				["G3", "8n"],
				["F3", "4n"],
				["D3", "2n"],
			],
		},
		{
			name: "Sweet Home Alabama",
			tempo: 84,
			artist: "Lynyrd Skynyrd",
			notes: [
				["D4", "8n"],
				["D4", "8n"],
				["D5", "16n"],
				["A4", "8n"],
				["D4", "16n"],
				["C4", "8n"],
				["C4", "8n"],
				["D5", "16n"],
				["G4", "8n"],
				["D4", "16n"],
				["G3", "8n"],
				["G3", "8n"],
				["G4", "8n."],
				["G4", "16n"],
				["A3", "16n"],
				["B3", "16n"],
				["D4", "16n"],
				["E4", "16n"],
				["D4", "16n"],
				["B3", "16n"],
				["A4", "16n"],
				["G4", "16n"],
			],
		},
		{
			name: "U Can't Touch This",
			tempo: 130,
			artist: "MC Hammer",
			notes: [
				["D4", "4n"],
				["C4", "8n"],
				["B3", "8n"],
				["A3", "8n"],
				["A4", "8n"],
				["A4", "8n"],
				["E3", "8n"],
				["G3", "8n"],
				["G4", "8n"],
				["G4", "8n"],
				["B3", "8n"],
				["A3", "8n"],
			],
		},
		{
			name: "Careless Whisper",
			tempo: 70,
			artist: "George Michael",
			notes: [
				["C#5", "8n"],
				["B4", "16n"],
				["F#4", "8n"],
				["D4", "8n"],
				["C#5", "8n."],
				["B4", "16n"],
				["F#4", "8n"],
				["D4", "8n."],
				["A4", "8n"],
				["G4", "16n"],
				["D4", "8n"],
				["B3", "8n"],
				["A4", "8n."],
				["G4", "16n"],
				["D4", "4n"],
			],
		},
	];

	function createSynths() {
		globalVolume = new Tone.Volume(-6).toDestination();
		reverb = new Tone.Reverb(1.5).connect(globalVolume);

		const mallets = new Tone.Sampler({
			urls: {
				C2: "C2.wav",
				C3: "C3.wav",
				C4: "C4.wav",
				C5: "C5.wav",
			},
			baseUrl: "/mallets/",
			release: 1,
		}).connect(reverb);

		const plucks = new Tone.Sampler({
			urls: {
				C2: "C2.wav",
				C3: "C3.wav",
				C4: "C4.wav",
				C5: "C5.wav",
			},
			baseUrl: "/plucks/",
			release: 1,
		}).connect(reverb);

		const winds = new Tone.Sampler({
			urls: {
				C2: "C2.wav",
				C3: "C3.wav",
				C4: "C4.wav",
				C5: "C5.wav",
			},
			baseUrl: "/winds/",
			release: 1,
			curve: "exponential",
		}).connect(reverb);
		winds.volume.value = -6;

		synths = {
			mallets,
			plucks,
			winds,
		};
	}

	function createBuffers() {
		drones = {
			low: [
				new Tone.ToneAudioBuffer("/static/low_drone/C1.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/low_drone/Cs1.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/low_drone/D1.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/low_drone/Ds1.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/low_drone/E1.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/low_drone/F1.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/low_drone/Fs1.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/low_drone/G1.wav", () => {
					bufferCounter++;
				}),
			],
			high: [
				new Tone.ToneAudioBuffer("/high_drone/C6.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/high_drone/Cs6.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/high_drone/D6.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/high_drone/Ds6.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/high_drone/E6.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/high_drone/F6.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/high_drone/Fs6.wav", () => {
					bufferCounter++;
				}),
				new Tone.ToneAudioBuffer("/high_drone/G6.wav", () => {
					bufferCounter++;
				}),
			],
		};
		incidentals = [
			new Tone.ToneAudioBuffer("/incidentals/1.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/2.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/3.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/4.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/5.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/6.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/7.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/8.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/9.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/10.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/11.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/12.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/13.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/14.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/15.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/16.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/17.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/18.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/19.wav", () => {
				bufferCounter++;
			}),
			new Tone.ToneAudioBuffer("/incidentals/20.wav", () => {
				bufferCounter++;
			}),
		];
	}

	function ambientLoop() {
		if (!finished) {
			const loopVolume = new Tone.Volume(-12).connect(globalVolume);

			function randomDrone(type) {
				return type === "low" ? drones.low[Math.floor(Math.random() * drones.low.length)] : drones.high[Math.floor(Math.random() * drones.high.length)];
			}

			function lowDroneLoop() {
				const drone = new Tone.Player(randomDrone("low")).connect(loopVolume);

				drone.fadeIn = 5;
				drone.fadeOut = 5;

				drone.start();

				// random time between 45000 and 55000 seconds to wait
				const timeToWait = Math.floor(Math.random() * 10000) + 45000;

				setTimeout(() => {
					lowDroneLoop();
				}, timeToWait);
			}

			function highDroneLoop() {
				const drone = new Tone.Player(randomDrone("high")).connect(loopVolume);

				drone.fadeIn = 5;
				drone.fadeOut = 5;
				drone.volume.value = -12;

				drone.start();

				// random time between 45000 and 55000 seconds to wait
				const timeToWait = Math.floor(Math.random() * 10000) + 45000;

				setTimeout(() => {
					highDroneLoop();
				}, timeToWait);
			}

			lowDroneLoop();
			highDroneLoop();
		}
	}

	function incidentalSounds() {
		if (!finished) {
			// random time between 500 and 2000 seconds to wait
			const timeToWait = Math.floor(Math.random() * 1500) + 500;

			setTimeout(() => {
				const chance = Math.random() * 100;

				if (chance < 8) {
					const incidental = new Tone.Player(incidentals[Math.floor(Math.random() * incidentals.length)]).connect(reverb);

					incidental.fadeIn = 0.25;
					incidental.volume.value = -12;

					incidental.start();
				}

				incidentalSounds();
			}, timeToWait);
		}
	}

	function generateMelody() {
		let melody = randomMelody();

		// choose a melody at random from the melodies array
		function randomMelody() {
			const values = Object.values(melodies);
			return values[Math.floor(Math.random() * values.length)];
		}

		// go through each note and potentially change the pitch. Make the likelihood of changing the pitch start at 0 and increase to 99% by the end of the melody
		function alterPitches(melody) {
			const alteredNotes = [];

			// change the pitch of a note by choosing a random pitch from the pitches array. Keep choosing until a different pitch is chosen.
			function changePitch(pitch) {
				const pitches = ["A", "A#", "B", "C", "C#", "D", "D#", "E", "F", "F#", "G", "G#"];
				let newPitch = pitch;

				while (newPitch === pitch) {
					newPitch = pitches[Math.floor(Math.random() * pitches.length)];
				}

				return newPitch;
			}

			// go through each note and potentially change the pitch
			melody.forEach(([note, duration], index) => {
				// grabs the pitch and accidental from the note, removing the register
				const pitch = note
					.split("")
					.filter((char) => isNaN(char))
					.join("");
				const register = note[note.length - 1];
				// calculate the likelihood of changing the pitch
				const likelihood = (index / melody.length) * 50;

				// if it's the first note or the likelihood is met, add the note to the alteredNotes array. Otherwise, change the pitch
				if (index === 0 || note === "rest" || (Math.random() * 100 >= likelihood && alteredNotes[index - 1][alteredNotes[index - 1].length - 1] !== "pitch changed")) {
					alteredNotes.push([note, duration, "pitch unchanged"]);
				} else {
					alteredNotes.push([`${changePitch(pitch)}${register}`, duration, "pitch changed"]);
				}
			});

			// remove likelihood and pitch changed status from the alteredNotes array
			alteredNotes.forEach((note) => {
				note.pop();
			});

			return alteredNotes;
		}

		// go through each note and potentially change the duration. Make the likelihood of changing the duration start at 0 and increase by a certain percentage by the end of the melody
		function alterDurations(melody) {
			const alteredNotes = [];

			function changeDuration(duration) {
				const durations = ["2n", "4n", "8n", "16n", "4n.", "8n.", "16n."];
				let newDuration = duration;

				while (newDuration === duration) {
					newDuration = durations[Math.floor(Math.random() * durations.length)];
				}

				return newDuration;
			}

			melody.forEach(([note, duration], index) => {
				const likelihood = (index / melody.length) * 25;

				if (index === 0 || (Math.random() * 100 >= likelihood && alteredNotes[index - 1][alteredNotes[index - 1].length - 1] !== "duration changed")) {
					alteredNotes.push([note, duration, "duration unchanged"]);
				} else {
					alteredNotes.push([note, changeDuration(duration), "duration changed"]);
				}
			});

			alteredNotes.forEach((note) => {
				note.pop();
			});

			return alteredNotes;
		}

		// go through each note and potentially remove it. Make the likelihood of removing a note start at 0 and increase by a certain percentage by the end of the melody
		function truncateMelody(melody) {
			const truncatedNotes = [];

			melody.forEach(([note, duration], index) => {
				const likelihood = (index / melody.length) * 100;

				if (index === 0 || (Math.random() * 100 >= likelihood && truncatedNotes[truncatedNotes.length - 1][truncatedNotes[truncatedNotes.length - 1].length - 1] !== "note removed")) {
					truncatedNotes.push([note, duration, "note kept"]);
				}
			});

			truncatedNotes.forEach((note) => {
				note.pop();
			});

			return truncatedNotes;
		}

		// applies a random alteration to the melody. There is a 1% chance that the melody will remain unchanged
		function alterMelody(melody) {
			const chance = Math.random() * 100;

			if (chance < 40) {
				return alterPitches(melody);
			} else if (chance >= 40 && chance < 70) {
				return alterDurations(melody);
			} else if (chance >= 70 && chance < 99) {
				return truncateMelody(melody);
			} else {
				return melody;
			}
		}

		// alters the tempo of the melody by a random amount. There is a 1% chance that the tempo will remain unchanged
		function alterTempo(tempo) {
			const chance = Math.random() * 100;

			if (chance < 99) {
				const alteration = Math.floor(Math.random() * (tempo * 0.25));
				const direction = Math.random() < 0.5 ? -1 : 1;
				const newTempo = tempo + alteration * direction;

				return newTempo;
			} else {
				return tempo;
			}
		}

		function playMelody() {
			if (!finished) {
				const notes = melody.notes;
				const tempo = melody.tempo;
				const sampler = pickSampler();

				let time = 0;
				let alteredNotes = notes;
				let alteredTempo = tempo;

				alteredNotes = alterMelody(alteredNotes);
				alteredTempo = alterTempo(alteredTempo);

				function pickSampler() {
					const samplers = Object.keys(synths);
					return synths[samplers[Math.floor(Math.random() * samplers.length)]];
				}

				function noteLengthInSeconds(duration) {
					switch (duration) {
						case "2n":
							return (2 * 60) / alteredTempo;
						case "4n":
							return (1 * 60) / alteredTempo;
						case "8n":
							return (0.5 * 60) / alteredTempo;
						case "16n":
							return (0.25 * 60) / alteredTempo;
						case "4n.":
							return (1.5 * 60) / alteredTempo;
						case "8n.":
							return (0.75 * 60) / alteredTempo;
						case "16n.":
							return (0.375 * 60) / alteredTempo;
						default:
							return 0;
					}
				}

				function generateAnalysis() {
					const truncation = {};
					const duration = {};
					const pitches = {};

					function analyzeTruncation() {
						if (notes.length !== alteredNotes.length) {
							truncation.truncated = true;
							truncation.originalLength = notes.length;
							truncation.alteredLength = alteredNotes.length;
						}
					}

					function analyzeDurations() {
						if (notes.length === alteredNotes.length) {
							for (let i = 0; i < notes.length; i++) {
								if (notes[i][1] !== alteredNotes[i][1]) {
									duration.altered = true;
									duration.firstChange = i;
									duration.alteredFragment = alteredNotes.slice(i, notes.length);
								}
							}
						}
					}

					function analyzePitches() {
						if (notes.length === alteredNotes.length) {
							for (let i = 0; i < notes.length; i++) {
								if (notes[i][0] !== alteredNotes[i][0]) {
									pitches.altered = true;
									pitches.firstChange = i;
									pitches.alteredFragment = alteredNotes.slice(i, alteredNotes.length);
								}
							}
						}
					}

					analyzeTruncation();
					analyzeDurations();
					analyzePitches();

					analysis.push({
						timestamp: new Date().toLocaleString(),
						melodyName: melody.name,
						originalMelody: notes,
						originalTempo: tempo,
						alteredTempo: alteredTempo,
						alteredMelody: alteredNotes,
						truncation: truncation,
						duration: duration,
						pitches: pitches,
					});
				}

				function showNoteSliver() {
					const opacity = 0.05;

					noteSlivers[Math.floor(Math.random() * noteSlivers.length)] = opacity;

					noteSlivers = [...noteSlivers];
				}

				alteredNotes.forEach(([note, duration], index) => {
					const durationInSeconds = noteLengthInSeconds(duration);

					setTimeout(() => {
						if (index === 0) {
							generateAnalysis();
						}

						for (let i = 0; i < noteSlivers.length; i++) {
							noteSlivers[i] = 0;
						}
						noteSlivers = [...noteSlivers];

						if (note !== "rest") {
							sampler.triggerAttackRelease(note, durationInSeconds);
							showNoteSliver();
						}

						if (index === alteredNotes.length - 1) {
							const timeToWait = Math.floor(Math.random() * 8000) + 3000;

							setTimeout(() => {
								for (let i = 0; i < noteSlivers.length; i++) {
									noteSlivers[i] = 0;
								}
								noteSlivers = [...noteSlivers];
							}, durationInSeconds * 1000);

							setTimeout(() => {
								generateMelody();
							}, timeToWait);
						}
					}, time * 1000);

					time += durationInSeconds;
				});
			}
		}

		playMelody();
	}

	function initializeAudio() {
		opacities.title = 0;

		setTimeout(async function () {
			await Tone.start();
			audioStarted = true;
			createSynths();
			createBuffers();
		}, 500);
	}

	function pickBackground() {
		// pick a random color from the backgroundPalette array excluding index 0
		if (!finished) {
			const index = Math.floor(Math.random() * (backgroundPalette.length - 1)) + 1;
			background = backgroundPalette[index];

			setTimeout(() => {
				pickBackground();
			}, 20000);
		}
	}

	function formatTime(time) {
		const minutes = Math.floor(timer / 60);
		const seconds = timer % 60;

		return `${minutes}:${seconds < 10 ? `0${seconds}` : seconds}`;
	}

	function finalizeAnalysis() {
		if (!username) {
			alert("Please enter your name before downloading the analysis file.");
			return;
		} else {
			const analysisFile = JSON.stringify(analysis);
			const blob = new Blob([analysisFile], { type: "application/json" });
			const url = URL.createObjectURL(blob);
			const link = document.createElement("a");
			link.href = url;
			link.download = "analysis-for-forrest.json";
			link.click();
			URL.revokeObjectURL(url);
		}
	}

	function startPiece() {
		opacities.play = 0;
		opacities.noteSlivers = 1;
		opacities.time = 1;
		bufferCounter = 0;
		playing = true;

		const timeToWait = Math.floor(Math.random() * 16000) + 6000;

		setTimeout(() => {
			pickBackground();
		}, 1000);

		setTimeout(() => {
			ambientLoop();
			incidentalSounds();
			timeKeeper();

			function timeKeeper() {
				timer++;

				setTimeout(() => {
					timeKeeper();
				}, 1000);
			}
		}, 3000);

		setTimeout(() => {
			generateMelody();
		}, timeToWait);

		setTimeout(() => {
			globalVolume.volume.linearRampTo(-Infinity, 10);
			opacities.noteSlivers = 0;
			opacities.time = 0;

			setTimeout(() => {
				background = backgroundPalette[0];

				setTimeout(() => {
					finished = true;
					setTimeout(() => {
						opacities.finished = 1;
					}, 1000);
				}, 1000);
			}, 10000);
		}, 300000);
	}

	$: if (bufferCounter === 36) {
		setTimeout(() => {
			opacities.play = 1;
		});
	}

	onMount(() => {
		opacities.title = 1;
		opacities.homepage = 1;
	});
</script>

<main class="d-flex min-vh-100 flex-column justify-content-center align-items-center text-light position-relative" style="background: {background};">
	<div class="homepage position-absolute top-0 end-0 m-3" style="opacity: {opacities.homepage};">
		<a class="text-light" href="https://forrestbalman.com" target="_blank">forrestbalman.com</a>
	</div>
	{#if !audioStarted}
		<div class="container d-flex flex-column justify-content-center align-items-center position-relative">
			<div class="title d-flex flex-column justify-content-center align-items-center gap-4" style="opacity: {opacities.title};">
				<h1 class="display-1 text-center user-select-none">I could've sworn I've heard this somewhere else... 🤔</h1>
				<button class="btn btn-light btn-lg" on:click="{initializeAudio}">Load Audio</button>
			</div>
		</div>
	{:else if bufferCounter !== 36 && !playing}
		<div class="container d-flex flex-column justify-content-center align-items-center">
			<h4 class="display-4 text-center user-select-none text-light">Loading...</h4>
			<div class="progress w-100">
				<div class="progress-bar bg-warning" role="progressbar" style="width: {(bufferCounter / 36) * 100}%;" aria-valuenow="{(bufferCounter / 36) * 100}" aria-valuemin="0" aria-valuemax="100"></div>
			</div>
		</div>
	{:else if finished}
		<div class="container min-vh-100 d-flex flex-column pt-4 text-center" style="opacity: {opacities.finished};">
			<h1 class="display-1 user-select-none text-light">I could've sworn I've heard this somewhere else... 🤔</h1>
			<h4 class="display-5 user-select-none text-light mb-5">Thanks for listening 👂</h4>
			<div class="text-start">
				<h4 class="mb-4">Follow the steps below to send me your analysis file</h4>
				<ol>
					<li class="mb-3">Type your <span class="fw-bold">first name</span> in the text field <input type="text" class="ms-3 rounded-2 border-0 p-1" bind:value="{username}" /></li>
					<li class="mb-3">Click the "Download Analysis" button <button class=" ms-3 btn btn-light" on:click="{finalizeAnalysis}">Download Analysis</button></li>
					<li>Attach the file to an email and send it to <a href="mailto:fbalman@ucsc.edu">fbalman@ucsc.edu</a></li>
				</ol>
			</div>
		</div>
	{:else}
		<div class="time position-absolute top-0 start-0 m-3" style="opacity: {opacities.time};">
			<p class="user-select-none text-light font-monospace">{formatTime(timer)}</p>
		</div>
		<div class="note-slivers position-absolute top-0 start-0 w-100 h-100 z-1 d-flex flex-column" style="opacity: {opacities.noteSlivers};">
			{#each noteSlivers as note}
				<div class="w-100 bg-white" style="height: {100 / noteSlivers.length}%; opacity: {note}; transition: opacity 0.3s ease;"></div>
			{/each}
		</div>
		<button class="btn btn-light btn-lg z-2" style="opacity: {opacities.play};" on:click="{startPiece}" disabled="{bufferCounter !== 36}">Play</button>
	{/if}
</main>

<style>
	* {
		transition: opacity 0.5s ease;
	}

	main {
		transition: background 2s ease;
	}
</style>
