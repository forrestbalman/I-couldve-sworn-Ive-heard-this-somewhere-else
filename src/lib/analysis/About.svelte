<script>
	import { base } from "$app/paths";
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

	const sources = [
		{
			name: "Arya's computer",
			source: aryaComputer,
		},
		{
			name: "Arya's phone",
			source: aryaPhone,
		},
		{
			name: "Emerson's computer",
			source: emersonComputer,
			color: "#800080",
		},
		{
			name: "Forrest's computer",
			source: forrestComputer,
			color: "#E6E6FA",
		},
		{
			name: "Forrest's tablet",
			source: forrestTablet,
			color: "#E6E6FA",
		},
		{
			name: "Kolawole's phone",
			source: kolawolePhone,
		},
		{
			name: "Lauren's computer",
			source: laurenComputer,
		},
		{
			name: "Michael's computer",
			source: michaelComputer,
		},
		{
			name: "Nicol's computer",
			source: nicolComputer,
			color: "#FF69B4",
		},
		{
			name: "Nicol's phone",
			source: nicolPhone,
			color: "#FF69B4",
		},
		{
			name: "Siamak's computer",
			source: siamakComputer,
		},
		{
			name: "Tyler's computer",
			source: tylerComputer,
		},
		{
			name: "Tyler's phone",
			source: tylerPhone,
		},
		{
			name: "Vidula's computer",
			source: vidulaComputer,
		},
	];
	const earlyMelodies = findMelodiesThatPrecedePerformance();

	function findMelodiesThatPrecedePerformance() {
		function extractTime(string) {
			const time = string.match(/\d{2}:\d{2}:\d{2}/);
			return time ? time[0] : null;
		}

		const firstMelodies = [];
		for (const source of sources) {
			for (const s of source.source) {
				const time = extractTime(s.timestamp)
					.split(":")
					.map((n) => Number(n));

				if (time[1] < 15) {
					firstMelodies.push({
						name: source.name,
						time: time,
					});
				}

				if (time[1] === 15 && time[2] <= 43) {
					firstMelodies.push({
						name: source.name,
						time: time,
					});
				}
			}
		}

		for (const e of firstMelodies) {
			e.time = e.time
				.map((n, i) => {
					if (i === 2 && n < 10) {
						return `0${n}`;
					} else {
						return n;
					}
				})
				.join(":");
		}

		return firstMelodies;
	}
</script>

<section id="about" class="container min-vh-100 py-5">
	<h1 class="text-center mb-4">About the piece: inspiration, methods, and other peeks behind the curtain 🔍</h1>
	<div class="mb-5">
		<h4>Overview. Tl;dr, copyright is ambiguous, and composers that use chance could be at risk</h4>
		<p>
			<span class="fst-italic">I could've sworn I've heard this somewhere else...</span>🤔 is a proof-of-concept piece that I composed to illustrate some of the potential legal issues with algorithmically generated music. More specifically, algorithmic music that uses chance operations to create its content.
		</p>
		<figure class="ms-3">
			<blockquote class="blockquote fs-6">
				The copyright owner of a musical work has the rights to make and distribute copies of it, publicly perform or display it, and make derivative works from it (including interpolations, remixes, or even videos using the musical work). Anyone else who wants to do these things must either get a license from
				the copyright owner, use a statutory license, or have an exemption apply, like fair use.
			</blockquote>
			<figcaption class="blockquote-footer">copyright.gov</figcaption>
		</figure>
		<p>
			The above quote from the U.S. Copyright Office's website is broad, but clear regarding music that's composed and consumed in traditional ways. Generally speaking, if you own the rights to a piece of music, you have the right to do whatever you want with it. However, what happens when a piece is created
			coincidentally?
		</p>
	</div>
	<div class="mb-5">
		<h4>Why (copy)write a piece like this?</h4>
		<p>
			Chance is a crucial element in all of my music. Whether it's by how a performer interprets my engraved notation, or by programming the electronics in my piece to playback sound indeterminately, not only does incorporating chance make every iteration, playback, or performance of a piece unique, but it also
			creates coincidental sound combinations/sequences that, likely, wouldn't be possible for the composer to intend.
		</p>
		<figure>
			<img class="img-fluid" src="{base}/notation.png" alt="An excerpt with some notation from my piece Have You Ever Wondered Why the Mole People March? for double bass" />
			<figcaption style="font-size: 14px; color: rgb(108, 117, 125);">An excerpt with some notation from my piece <span class="fst-italic">Have You Ever Wondered Why the Mole People March?</span> for double bass that illustrates chance.</figcaption>
		</figure>
		<p>
			One series of sounds that a composer might not've intended when using chance in their writing is a recreation a copyrighted melody. If it wasn't the composer's intention, does it fall into fair use? If a court rules that if the composition can generate copyrighted content, does that mean future compositions
			need to take this into account?
		</p>
	</div>
	<div class="mb-5">
		<h4>Before we talk about methods + code, what's kinda algorithms are we talking about? 🧠</h4>
		<p>
			The most important thing I want to be taken into consideration is that this piece is caricature of a more nuanced issue. The methods, including the code and source material, are all based on well-known, relatively easy-to-recognize (at least to a Western audience), and most importantly <span class="fw-bold"
				>copyrighted</span> melodies.
		</p>
		<p>Here's some Javascript code that will generate a random melody with the following constraints:</p>
		<ul>
			<li>7 - 12 notes in length</li>
			<li>Each note can be a half, quarter, eighth, or sixteenth note, or rest</li>
			<li>Pitches will be from an equally tempered chromatic scale from C4 to C5</li>
		</ul>
		<figure>
			<img class="img-fluid" src="{base}/code-1.png" alt="Javascript code that generates a random melody" />
			<figcaption style="font-size: 14px; color: rgb(108, 117, 125);">Javascript code that generates a random melody</figcaption>
		</figure>
		<p>
			This is a very simple example with several limitations; however, there are many melodies (or melodic fragments) that this program could produce notation to playback. Speaking from my experience, algorithms that are used for generating any kind of melodic content have more variety influencing any output;
			however, the above example can serve as a skeleton for generating content.
		</p>
	</div>
	<div class="mb-5">
		<h4 class="mb-4">Alright, now we can talk about methods + code</h4>
		<h5>Breaking down the sound world 🌎</h5>
		<p>
			During my junior year of my Bachelor's, my advisor told me something along the lines of, "If all your piece is doing is playing back numbers, it's not music... it's a science experiment." While I don't agree with it entirely/literally, there is truth behind the "artist's touch" impacting the music in such a
			way that makes it theirs. If two composers sit down and program the same algorithm to output the same content, there's little to no sense of originality in the output. However, I wanted this piece to serve as an accessory to the (somewhat) scientific research I'm conducting for this class. So, the
			compositional choices I made aimed to reconcile my aesthetic and research goals
		</p>
	</div>
	<div class="mb-3">
		<h5>Texture and incidental sounds</h5>
		<p>
			I almost always use electronic sounds as an accompaniment or textural element of a piece. Sometimes they're based on a direct response to the inspiration/concept behind a piece. Other times, it is purely vibe/intuition based. That being said, in addition to the melodies that play throughout the piece, I
			added two textural layers to the sound world:
		</p>
	</div>
	<div class="mb-3">
		<h5>Ambient Loop</h5>
		<div class="d-flex gap-5 mb-3">
			<div class="d-flex flex-column justify-content-between w-100">
				<div>
					<h6>Low drone</h6>
					<p>The low drone is a metallic-inspired, grainy, synth that can occur as a random note between C1 and G1</p>
				</div>
				<audio controls class="w-100">
					<source src="{base}/low_drone/C1.wav" />
					Your browser does not support the audio element.
				</audio>
			</div>
			<div class="d-flex flex-column justify-content-between w-100">
				<div>
					<h6>High drone</h6>
					<p>The high drone is a breathy, tinny, synth that can occur as a random note between C6 and G6</p>
				</div>
				<audio controls class="w-100">
					<source src="{base}/high_drone/C6.wav" />
					Your browser does not support the audio element.
				</audio>
			</div>
		</div>
		<p>
			The below code snippet mostly consists of instructions that tell the program to "loop" the ambiences by cross-fading two sound files at certain points. This is because of challenges I've faced trying to create seamless loops of complex sounds in the browser. Dovetailing and cross-fading has been the most
			effective approach for me. Other than the "looping" logic, all the function does is:
		</p>
		<ul>
			<li>Tell the sound files to fade in and out over 5 seconds</li>
			<li>Sets their initial volumes to -12dB</li>
			<li>and, Chooses a random note to play.</li>
		</ul>
		<img class="img-fluid" src="{base}/code-2.png" alt="A code block showing the looping logic" />
	</div>
	<div class="mb-3">
		<h5 class="mt-3">Incidental sounds</h5>
		<p>Every once and a while, the piece will play a curated, but arbitrary sound. There is a bank of 20 sounds the piece will choose from at any given time. These were all found on my hard drive as part of sound libraries I've collected over the years.</p>
		<div class="d-flex flex-wrap justify-content-between align-items-center gap-2 mb-3">
			{#each new Array(20) as _, i}
				<audio controls style="max-width: 200px;">
					<source src="{base}/incidentals/{i + 1}.wav" />
					Your browser does not support the audio element.
				</audio>
			{/each}
		</div>
		<p>
			Because the incidental sounds aren't being looped, the code regarding their playback is pretty straightforward. Every 0.5 to 1.5 seconds, there's an 8% chance that the piece will play one of the 20 sounds. the <code>if (!finished)</code> at the top of the function just checks to see if the piece is currently
			playing in order to prevent the incidental sounds from playing after the piece has finished.
		</p>
		<ul>
			<li>Tell the sound files to fade in and out over 5 seconds</li>
			<li>Sets their initial volumes to -12dB</li>
			<li>and, Chooses a random note to play.</li>
		</ul>
		<img class="img-fluid" src="{base}/code-3.png" alt="A code block showing the incidental sound logic" />
	</div>
	<div class="mb-3">
		<h5>From notation to playback. The melody generation process. 🎼</h5>
		<p>The programming behind the piece followed a 4 step process to generate melodies:</p>
		<ol>
			<li>Notating the melodies in a way the Javascript library I used for audio playback understands.</li>
			<li>Picking a melody at random from the list of notated melodies.</li>
			<li>Choosing to apply a modification to the melody or not, then modifying the melody if it was chosen to be modified.</li>
			<li>Playing back the melody using a bank of curated samples.</li>
		</ol>
	</div>
	<div class="mb-3">
		<h5>Melodic source materials</h5>
		<p>When composing this piece, I took creative liberties regarding any melodic transformations; however, the source material is all pre-notated using my transcriptions of 14 "well-known" melodies, which are:</p>
		<ul>
			<li class="fst-italic">Stairway to Heaven</li>
			<li class="fst-italic">Billie Jean</li>
			<li class="fst-italic">What a Wonderful World</li>
			<li class="fst-italic">Sweet Child O' Mine</li>
			<li class="fst-italic">Hey Jude</li>
			<li class="fst-italic">Never Gonna Give You Up</li>
			<li class="fst-italic">Livin' on a Prayer</li>
			<li class="fst-italic">Dancing Queen</li>
			<li class="fst-italic">Don't Stop Believin'</li>
			<li class="fst-italic">The Final Countdown</li>
			<li class="fst-italic">Y.M.C.A</li>
			<li class="fst-italic">Sweet Home Alabama</li>
			<li class="fst-italic">U Can't Touch This</li>
			<li class="fst-italic">Careless Whisper</li>
		</ul>
	</div>
	<div class="mb-3">
		<h5>Notating a melody</h5>
		<p>All of the melodic playback is being handled by a Javascript library called Tone.js . When coding instructions for audio playback, a pitch and rhythm must be provided. In my case, I chose to notate pitch using pitch-register notation, and I chose to notate rhythm using BPM/time signature relative values.</p>
		<p>The code snippet below shows an example melody's "meta-data", including the melodic notation.</p>
		<img class="img-fluid" src="{base}/code-4.png" alt="A code block showing the meta-data for a melody" />
	</div>
	<div class="mb-3">
		<h5>Melodic alteration</h5>
		<p>
			Because I wanted the piece to (somewhat) simulate a musical environment where there's a likelihood of hearing something recognizable, and potentially copyrighted, in most cases, each melody is transformed in some way before it reaches the playback stage. And, because of the extreme unlikelihood of a
			copyrighted melody to be played back through several layers of chance, I gave the program a 1% chance to let a melody pass through the code unaltered. When a melody is transformed, it undergoes 1 of 3 possible alterations, and an almost guaranteed tempo alteration.:
		</p>
		<ul>
			<li>Pitch alteration. The pitch changes, but the register stays the same.</li>
			<li>Rhythm alteration. The note value changes to a half, quarter, eighth, sixteenth, or dotted duration.</li>
			<li>Truncation. The melody stops after a certain point</li>
			<li>Tempo alteration. There is a 99% chance that a melody will be played back at a tempo 20BPM faster or slower.</li>
		</ul>
		<p>In order to stay consistent with the original idea of the piece, which revolves around thinking that you might've encountered this melody before, the transformations follow a couple parameters:</p>
		<ul>
			<li>The transformation is more likely to occur the later into the melody you get. This usually ensures that the melody starts off recognizable, but ends up unrecognizable</li>
			<li>Transformations apply to all notes after the first transformation. In other words, the only time a melody will change one note is if that note is the last note.</li>
		</ul>
		<p>
			The below code block is for altering pitches. It runs until it selects a random pitch from the pitches array, then returns the pitch. Then, for every note in the melody, based on the likelihood which is influenced by how early/late the note is in the melody, change the pitch. If the pitch before the current
			pitch is flagged as "changed", the current pitch is changed by default (no chance necessary). The only section that might be unintuitive is the part that defines <code>pitch</code>, which uses, <code>split()</code>, <code>filter()</code>, and <code>join()</code>. All this does is remove the register from
			the note, because all the function is concerned with is the pitch.
		</p>
		<img class="img-fluid mb-3" src="{base}/code-5.png" alt="A code block showing the pitch alteration logic" />
		<p>The next code block alters durations. If you compare it to the pitch alteration function, you'll notice that they're mostly similar. The only main difference is that instead of selecting a pitch from an array of pitches, this function selects a rhythm from an array of rhythms.</p>
		<img class="img-fluid mb-3" src="{base}/code-6.png" alt="A code block showing the rhythm alteration logic" />
		<p>
			The next transformation truncates the melody after a certain point. This is the simplest of the transformation to visualize. It follows the same logic that the previous transformations use in terms of flagging a note as the first note to undergo a transformation, then applying a transformation to the rest
			of the notes in the melody; however, in this case, because the melody is being truncated, it just cuts it off after the first flagged note.
		</p>
		<img class="img-fluid mb-3" src="{base}/code-7.png" alt="A code block showing the truncation logic" />
		<p>
			The last alteration involves changing the melody's tempo. The reason why I chose to make this alteration mandatory, is due to slight tempo changes not having as profound of an impact on one's ability to recognize a melody. This function is very simple. It just adds or subtracts a number up to 25% of the
			original tempo of the melody.
		</p>
		<img class="img-fluid mb-3" src="{base}/code-8.png" alt="A code block showing the tempo alteration logic" />
	</div>
	<div class="mb-3">
		<h5>Melodic playback</h5>
		<p>
			The <code>playMelody()</code> function is responsible for taking the rhythm data from each note in the melody and converting it to a value in milliseconds (remember that each melody is originally notated using tempo-relative values). Because this part of the code is scheduling all of the melody to occur in advance,
			those schedules can also tied to other aspects of the piece that rely on time, like making a simple visualization, and generating an analysis file.
		</p>
	</div>
	<div class="mb-5">
		<h5>Samples</h5>
		<p>I decided to make use of 3 different samples for melodic playback.</p>
		<div class="d-flex gap-5 mb-3">
			<div class="d-flex flex-column justify-content-between w-100">
				<div>
					<h6>Mallet</h6>
					<p>A vibraphone inspired synth, that also has a sizzle-cymbalesque quality to it.</p>
				</div>
				<audio controls class="w-100">
					<source src="{base}/mallets/C4.wav" />
					Your browser does not support the audio element.
				</audio>
			</div>
			<div class="d-flex flex-column justify-content-between w-100">
				<div>
					<h6>Pluck</h6>
					<p>A heavily filtered, and slightly bit-crushed guitar inspired synth.</p>
				</div>
				<audio controls class="w-100">
					<source src="{base}/plucks/C4.wav" />
					Your browser does not support the audio element.
				</audio>
			</div>
			<div class="d-flex flex-column justify-content-between w-100">
				<div>
					<h6>Winds</h6>
					<p>A breathy wooden flute sample, similar to a filtered bansuri.</p>
				</div>
				<audio controls class="w-100">
					<source src="{base}/winds/C4.wav" />
					Your browser does not support the audio element.
				</audio>
			</div>
		</div>
		<p>The code below shows how a melody is converted for playback purposes, which involves translating the rhythms to millisecond values, and selecting a sample at random for playback.</p>
		<img class="img-fluid mb-3" src="{base}/code-9.png" alt="A code block showing the playback logic" />
		<p>
			Then, we schedule the playback event to occur at a point after the previous note finishes. The code that controls sound playback is <code>sampler.triggerAttackRelease()</code>, which is Tone.js' method for taking a sound source and playing it back for a defined length of time. In addition to general
			playback, the timing part of the code is also responsible for:
		</p>
		<ul>
			<li>Generating analysis data, but this only happens on the first note. Otherwise the same data for the whole melody would be generated with each note.</li>
			<li>Lighting up a random "note sliver", which is a horizontal slice of the screen that functions as a pitch visualizer during the piece's playback.</li>
			<li>Generating another melody after a delay, but this only happens on the last note. Otherwise a melody would trigger each time a note plays back.</li>
		</ul>
		<img class="img-fluid mb-3" src="{base}/code-10.png" alt="A code block showing the playback scheduling logic" />
	</div>
	<div class="mb-5">
		<h4>What does the data tell us? 🤔</h4>
		<p>This section is about what data I recorded. All of the data can be visualized in different ways in the later sections of the webpage.</p>
		<p>Because my original idea for transcription, sound-coding, and visualization revolved around seeing how each of the melodies differed from their original source material, I structured each analysis file to include:</p>
		<ul>
			<li>A timestamp that shows when the melody started playback.</li>
			<li>The name of the original melody.</li>
			<li>The notation for the original melody.</li>
			<li>The original tempo.</li>
			<li>The altered tempo.</li>
			<li>The notation for the altered melody.</li>
			<li>3 containers for each of the 3 "optional" melodic transformations. Each container reports: the note where the change occurs, and the fragment of the melody that consists of all of the alterations.</li>
		</ul>
		<p>Here's an example of a fragment from <span style="color: HotPink;">Nicol's phone</span></p>
		<img class="img-fluid mb-3" src="{base}/code-11.png" alt="A code block showing an example of a fragment from Nicol's phone" />
		<p>I think the best way to get anything meaningful from the data is to play around with it yourself. Use the sections later in the webpage to look at single sound sources, compare multiple, or look at the performance visualization.</p>
	</div>
	<div class="mb-3">
		<h4>Did we learn anything about copyright? ©️</h4>
		<p>Because music copyright uses broad language to cover situations involving music that's created/consumed using traditional idioms, I think it's best to look at the Fair Use Statute.</p>
		<figure class="ms-3">
			<blockquote class="blockquote fs-6">
				Section 107 of the Copyright Act gives examples of purposes that are favored by fair use: “criticism, comment, news reporting, teaching (including multiple copies for classroom use), scholarship, [and] research.” Use for one of these purposes is not automatically fair, and uses for other purposes can be
				fair. The statute lays out four factors to consider in deciding whether a particular use is fair.
			</blockquote>
			<figcaption class="blockquote-footer"><cite title="Penn State Copyright Information">Penn State Copyright Information</cite></figcaption>
		</figure>
		<p>This leads us to the "Four Factors of Fair Use"</p>
		<ol>
			<li>The purpose and character of the use, including whether such use is of a commercial nature or is for nonprofit educational purposes.</li>
			<li>The nature of the copyrighted work.</li>
			<li>The amount and substantiality of the portion used in relation to the copyrighted work as a whole.</li>
			<li>The effect of the use upon the potential market for or value of the copyrighted work.</li>
		</ol>
		<p>Using the Fair Use Statute as a legal metric for whether or not a piece like this would leave any of the artists/entities that own the rights to the melodies I used as source material for this piece, I think it's fair to say that I'm not going to be expecting any cease and desist letters anytime soon 🤣.</p>
		<p>
			However, like I mentioned at the beginning of this write up, this is a caricature piece that is designed to make an incredibly unlikely scenario much more likely in a somewhat controlled environment as a proof of concept. The question about how algorithmic composition pertains to copyright infringement is
			still up in the air.
		</p>
		<p>Putting my piece aside, and looking at algorithmic composition more broadly as a whole, what do the "Four Factors of Fair Use" have to say?</p>
	</div>
	<div class="mb-3">
		<h5>Purpose and character of the use</h5>
		<p>
			This is going to be completely unique based on the piece in question. If someone uses an algorithm to generate melodic content randomly, and it just so happens a recording gets a snippet of <span class="fst-italic">My Heart Will Go On</span>, YouTube might flag the video for removal, but it will most likely
			win any appeal due to the incidental nature of the content. However, if the algorithm uses large, recognizable samples from the piece without any effort to transform the source material, I think there's room to argue that the copyrighted piece in question might be entitled to some/all of the intellectual
			property. It really depends on the situation.
		</p>
	</div>
	<div class="mb-3">
		<h5>The nature of the copyrighted work</h5>
		<p>
			At first glance, this factor felt a little bit irrelevant. After all, if the copyrighted work is the intellectual property in question, what difference does it make that it makes an appearance in another work. All the really matters is the sound, right? Well, no... I think it's very crucial to distinguish
			the natures between the original work and the work that's referencing it. In the case of my work that uses algorithms to generate music output, the point isn't to create a parody, cover, or linear narrative. It's to repurpose the sound for my aesthetic goals, and if my goals are starkly different in
			comparison to the intentions of the original work, I agree those are grounds for fair use.
		</p>
	</div>
	<div class="mb-3">
		<h5>Amount and substantiality</h5>
		<p>
			Is the hook, or the intro of a popular melody enough to constitute a violation of the original work's intellectual property? This is definitely unique based on the work in question. Some popular songs consist of mostly repeated material, like in the case of <a
				href="https://www.youtube.com/watch?v=LKYPYj2XX80">Daft Punk's <span class="fst-italic">Around the World</span></a> (which also makes use of samples, by the way). What would be an amount that would raise a red flag? Does any amount of the original work give the copyright holder the right to take legal action,
			then the court decides if it's fair use?
		</p>
	</div>
	<div class="mb-5">
		<h5>Effect on potential market value</h5>
		<p>Is the traction gained by an algorithmically generated composition that likely has no connection to the original work going to have any impact on the original work's market value? I think it's safe to say that the answer is no in pretty much all cases, but you never know.</p>
	</div>
	<div id="timings" class="mb-5">
		<h4>Figuring out specific timings in the recording. ⌛</h4>
		<p>
			My iPhone's <span class="fst-italic">Voice Memos</span> only tells me recording start and end times to the nearest minute. That being said, the recording is exactly <span class="fw-bold">18 minutes and 20 seconds long</span>, so I figure the performance had to have started in the
			<span class="fw-bold">range of 10:10:40AM to 10:10:59AM</span>. For the sake of consistency, I'll assume that the recording started at 10:10:40AM.
		</p>
		<p>Upon examining the audio file closely in Ableton Live, the performance starts approximately when I say, "Go", which can be seen in the images below taking place approximately 5 minutes and 3 seconds into the recording. This suggests that the performance starts at 10:15:43AM.</p>
		<img class="img-fluid" src="{base}/ableton-1.png" alt="Ableton Live's timeline showing the start of the performance" />
		<img class="img-fluid mb-3" src="{base}/ableton-2.png" alt="Ableton Live's timeline showing the start of the performance" />
		<p>This presents an issue because when looking at the analysis files for some of the participants, melodies were first generated as early as 10:14.</p>
		<ul>
			{#each earlyMelodies as { name, time }}
				<li>{name} - {time}</li>
			{/each}
		</ul>
		<p>I'm assuming that there we're participants who started the piece in advance and faded their volume in when I said go. That being said, all melodies generated before 10:15:43AM won't be included in the timeline visualizations for individual participants.</p>
		<p>Looking at the audio recording, the performance ending after we all applauded, which occurs approximately 10 minutes and 18 seconds into the recording, making the length of the performance roughly 5 minutes and 15 seconds, suggesting that the performance ended at 10:20:57AM.</p>
	</div>
	<div class="mb-5">
		<h4>Visualizations and the problems that arose post performance 🫨</h4>
		<p>It turns out that creating a simple visualizer with the data I collected wasn't too difficult to create, in theory:</p>
		<ul>
			<li>Convert the timestamps from each event into seconds to compare with the relative length of the audio file.</li>
			<li>Render transcription information on the screen.</li>
		</ul>
		<p>For the <span class="fst-italic">performance</span> section of the field recording, I chose to separate the data into spoken and melody sections. Unfortunately, problems began to appear when hearing the transcribed information display as I listened to the playback.</p>
		<p>
			Each analysis file reported back information of when a melody was generated indiscriminate of whether or not the sound was audible in the recording. Why is this a problem. Well, with so many sound sources contributing to this performance, it becomes a challenge to hear certain melodies for a variety of
			reasons.
		</p>
		<ul>
			<li>Their distance from the microphone.</li>
			<li>The volume of the device playing back the melody.</li>
			<li>Computer hiccups that prevented the melody playing back, i.e. generating the data for playback, but not playing the sound.</li>
			<li>Generating sound, but not reporting the data back in the analysis file.</li>
			<li>Timestamps differing between computers because of slight discrepancies between their clocks.</li>
			<li>Reloading the piece at any point.</li>
		</ul>
		<p>
			If you listen to the beginning of the recording, you can hear a melody based on <span class="fst-italic">Sweet Home Alabama</span>, which happens to be coming from Arya's computer. The timestamp aligns with the recording, which functions almost like a "downbeat" when trying to sync the analysis files with
			the field recording.
		</p>
		<p>
			However, it doesn't take long before we start to hear (or not hear) a combination of factors that are present (or not present) in the recording despite being present (or not present) in the data. Certain melodies are reported a bit later than they occur, or aren't present at all in the data despite being a
			part of the recording.
		</p>
		<p>That being said, one potential, but nearly impossible approach would be to curate the data and filter out all non-present melodies. This would be an extreme (I would argue impossible) ear training exercise due to the complexity of the soundworld created by this particular performance.</p>
		<p>
			In this case, I'm willing to concede that there are a combination of factors (namely the ones listed earlier) that also include likely flaws in my code as far as data representation is concerned. I'm not worried about the data collection as a whole, because the process is bound to melody generation. When
			the content is created, the analysis file has content added to it. In other words, if you were able to successfully load the piece and play it back, there wouldn't be anything preventing the analysis file from being generated.
		</p>
		<p>I think adding additional information to the fields at the end of the recording for data collection would've made data potentially more accurate. Things like:</p>
		<ul>
			<li>Make and model of computer.</li>
			<li>Volume level.</li>
			<li>Distance from the microphone.</li>
		</ul>
		<p>But in a final reflection on some of the problematic results of the algorithmic content of the piece, I think they tie in to the overall theme of the nuance of algorithms.</p>
	</div>
	<div>
		<h4>In closing: Algorithms are human, too 👨‍🔬👩‍🔬</h4>
		<p>I closed the previous section by talking about algorithms having nuance. This is true in a lot of cases, like in the case of this piece; however, it isn't always true.</p>
		<p>
			In a computer science curriculum, a student will often have to take a pair of related courses: <span class="fst-italic">Data Structures</span> and <span class="fst-italic">Algorithms</span>. In this context, students learn about how digital information is structured, and conventions that have arose to
			manipulate that data (often the most efficiently). And because methods have been determined to handle data the most efficiently, the class becomes a matter of learning pre-established code, which isn't the most creative process.
		</p>
		<p>
			Compare that to an artistic application of code, like this piece. I wrote algorithms that took a set of pitches, in this case copyrighted melodies, and curated a series of transformations. I had complete control over note just how the transformations were implemented, but the likelihood of them occurring,
			as well. Bearing this in mind, and trying to stay somewhat connected to the discussion of algorithmic music and how it pertains to copyright, even in a case where pre-existing materials are being used as our source, not only are the transformed outcomes artistic interpretations capable of standing on their
			own, but the process of creating the code that generated the content is just as artistic.
		</p>
		<p>
			The term "algorithm," nowadays, is broadly applied to any digital system of recommendation. We imagine a super computer crunching numbers in a big warehouse somewhere in Silicon Valley, making decisions about what media will <span class="fw-bold">you</span> will consume at any given time. The reality is that
			algorithms have to, at least, start with human input. Engineers are designing these technologies, and with the case of algorithms, like the ones in my piece, and the ones used by media platforms to recommend content, there are variables that are taken into account by artists and data scientists alike to create
			a process that produces some type of outcome.
		</p>
		<p>
			Will the outcome be the same every time? No, probably not. Is each outcome different from person to person? Yes, tremendously in most cases. Have internet algorithms deployed the SkyNet already, and is humanity already doomed 😱? Absolutely, yes! Just kidding.... The fact remains that between the
			conversational (arguably misappropriated) use of the term algorithm suggests that the process itself was created by a means of automation, when in reality algorithms are not only an essential way in which we engage with content over the internet, but algorithms are also created by humans, engineers and
			artists alike.
		</p>
		<p>
			In my case, as the composer, yes, the algorithms generated the sounds, visuals, and all other content associated with this piece, but the computer <span class="fw-bold">did not</span> know how to do go about doing that until I programmed it to do so. As a composer of computer music that often uses algorithms
			for its content, my code functions comparably to a score for a member of a symphony orchestra. Just as much effort goes into the coding behind the scenes.
		</p>
		<p>
			In a final close, I'd like to say that algorithms have been endemic to music for a long time. Systems like scales, rhythmic motifs, phrases, chords, and other musical devices are not only algorithms in and of themselves, but they can also serve as fuel for other algorithmic processes that, in turn, generate
			more musical content (like in the case of this piece). Copyright definitions pertaining to music are broad, which is a good thing due to the importance of interpretation in art, but the language doesn't feel as if it truly encompasses how far music has come regarding the tools and resources we already have
			access to. If an algorithmically random piece of music gets flagged by YouTube's content vetting algorithm, will it likely fall under the guidelines of <span class="fst-italic">Fair Use</span>? Yes, but I don't think the discussion ends here, or there.
		</p>
		<ul>
			<li>
				Because computers are capable of hearing much more objectively than humans, there are some clear drawbacks to using the same system to compare a piece like mine, for instance, to a video that makes no attempt to transform the content. Should there be an alternative vetting process for "works of sonic
				art"?
			</li>
			<li>Computers can play back rhythms, pitches, timbres (really all aspects of sound) indiscriminately, who's gets to decide if any two melodies are identical if one uses slightly different sounds that can be detected by a computer, but not perceivable by humans?</li>
		</ul>
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
</style>
