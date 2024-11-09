<script>
	import { onMount } from "svelte";
	import { base } from "$app/paths";
	import { fade } from "svelte/transition";

	const spoken = [
		{
			time: 0,
			speaker: "Vidula",
			words: "... we didn't do anything. 😂",
		},
		{
			time: 2,
			speaker: "Forrest",
			words: "What melody? What do you mean?",
		},
		{
			time: 3,
			speaker: "Vidula",
			words: "Like, when you were saying... it's like my analysis, or like, you know?",
		},
		{
			time: 6.5,
			speaker: "Vidula",
			words: "We had to put our name... yeah, but that could be (unsure).",
		},
		{
			time: 7.75,
			speaker: "Forrest",
			words: "Yeah.",
		},
		{
			time: 10.25,
			speaker: "Forrest",
			words: "Well, you performed the piece. Or, your hardware did.",
		},
		{
			time: 11.6,
			speaker: "Vidula",
			words: "That's what I, eh, how!? 😂",
		},
		{
			time: 12.8,
			speaker: "Michael",
			words: "Your hard drive performed it.",
		},
		{
			time: 13.6,
			speaker: "Forrest",
			words: "Well I could... well, the piece couldn't've, eh, happened without you.",
		},
		{
			time: 16,
			speaker: "Vidula",
			words: "😂",
		},
		{
			time: 16.5,
			speaker: "Vidula",
			words: "That's... scary?",
		},
		{
			time: 16.75,
			speaker: "Forrest",
			words: "Without any of us, so...",
		},
		{
			time: 18.25,
			speaker: "Class",
			words: "😂😂😂",
		},
		{
			time: 19.25,
			speaker: "Vidula",
			words: "Pointless? I don't know... 😂 because just yesterday, just last week, we were like, talking about alienation. This feels alienation.",
		},
		{
			time: 26.75,
			speaker: "Nicol",
			words: "Uh huh.",
		},
		{
			time: 27.1,
			speaker: "Vidula",
			words: "😂, because, when your prac... like, this music, uhh.",
		},
		{
			time: 32.2,
			speaker: "Vidula",
			words: "... example came up like when you're actually practicing music, or painting, you're actually... also shaping you as a musician?",
		},
		{
			time: 40.2,
			speaker: "Nicol",
			words: "Uh huh.",
		},
		{
			time: 40.25,
			speaker: "Vidula",
			words: "... but when that has not happened... the alienation will occur. (???)",
		},
		{
			time: 43.6,
			speaker: "Forrest",
			words: "This doesn't shape you as a musician?",
		},
		{
			time: 45.25,
			speaker: "Class",
			words: "😂😂",
		},
		{
			time: 46.4,
			speaker: "Vidula",
			words: "I mean, I'm not doing anything!",
		},
		{
			time: 48.4,
			speaker: "Forrest",
			words: "I don't know... I mean, I feel like, with...",
		},
		{
			time: 50.3,
			speaker: "Vidula",
			words: "Literally, I was just like, rec... receiving it.",
		},
		{
			time: 52.9,
			speaker: "Forrest",
			words: "That's kinda why... computers are great, because we don't have to do anything.",
		},
		{
			time: 57.5,
			speaker: "Forrest",
			words: "Or, the... why it's not necessarily that we're doing anything, the computer's a tool to help automate a process that we would otherwise be doing.",
		},
		{
			time: 57.6,
			speaker: "Class",
			words: "😂😂😂",
		},
		{
			time: 57.7,
			speaker: "Vidula",
			words: "Okay, that's scary... 😂",
		},
		{
			time: 64,
			speaker: "Michael",
			words: "Mmm... Okay, yeah.",
		},
		{
			time: 64.2,
			speaker: "Vidula",
			words: "It's like why, for instance, like, writing... If I'm not writing... myself, I'm not making myself into a writer.",
		},
		{
			time: 67.75,
			speaker: "Vidula",
			words: "Like, of course, AI can write everything for me, but... I'm not becoming a writer.",
		},
		{
			time: 73,
			speaker: "Forrest",
			words: "Sure.",
		},
		{
			time: 75.8,
			speaker: "Vidula",
			words: "I'm getting the assignments written, but... I'm not the writer.",
		},
		{
			time: 80.5,
			speaker: "Forrest",
			words: "It's just... think of... think...",
		},
		{
			time: 80.75,
			speaker: "Vidula",
			words: "I'm not shaping myself as a writer.",
		},
		{
			time: 82.4,
			speaker: "Forrest",
			words: "Well think... start thinking of your laptop as an instrument.",
		},
		{
			time: 84.6,
			speaker: "Vidula",
			words: "Mmm...",
		},
		{
			time: 85.1,
			speaker: "Nicol",
			words: `So, Roland Barthes would say that "when you read, you are a writer."`,
		},
		{
			time: 89.5,
			speaker: "Michael",
			words: "Mmm...",
		},
		{
			time: 90,
			speaker: "Vidula",
			words: `But, again, I'm reading, like, but... with AI... I'm just letting AI write for me, I don't even have to read it...`,
		},
		{
			time: 90,
			speaker: "Nicol",
			words: `Death of the Author.`,
		},
		{
			time: 98.4,
			speaker: "Vidula",
			words: `I just have to print it and (???), just like.`,
		},
		{
			time: 100.5,
			speaker: "Forrest",
			words: `I get what you're saying, but in... in my defense, this is not AI.`,
		},
		{
			time: 101.6,
			speaker: "Vidula",
			words: `Yeah...`,
		},
		{
			time: 103.3,
			speaker: "Vidula",
			words: `No! I'm not e... I'm not saying you should defend or anything, but I'm in a...`,
		},
		{
			time: 107.5,
			speaker: "Class",
			words: `😂`,
		},
		{
			time: 107.75,
			speaker: "Vidula",
			words: `I don't know! I can't call it, like, my melody... 😂. The way you're saying. You... I created.`,
		},
		{
			time: 110.5,
			speaker: "Forrest",
			words: `Well...`,
		},
		{
			time: 112,
			speaker: "Forrest",
			words: `Well, I mean, t... it... it... it was played from your hardware, and generated by your computer, so I guess there is some... kind of... artistic sense of ownership there.`,
		},
		{
			time: 120,
			speaker: "Vidula",
			words: `How so? Like... eh, because the way I use, uh... my laptop, does it shape how the melody's created.`,
		},
		{
			time: 127.8,
			speaker: "Forrest",
			words: `No, but your laptop... but based on the fact that your laptop ran the piece at that point in time, with everything else going on, it's unique.`,
		},
		{
			time: 135.8,
			speaker: "Kolawole",
			words: `Yeah, anything for initiating that she also has an input... she initiated the process of it's playing.`,
		},
		{
			time: 141.4,
			speaker: "Class",
			words: `Yeah.`,
		},
		{
			time: 141.9,
			speaker: "Forrest",
			words: `And you consented to being part of the performance, so...`,
		},
		{
			time: 141.9,
			speaker: "Tyler",
			words: `If you...`,
		},
		{
			time: 144.5,
			speaker: "Tyler",
			words: `If you weren't here, it wouldn't have been the same.`,
		},
		{
			time: 146.4,
			speaker: "Forrest",
			words: `Exactly.`,
		},
	];
	let audio, loaded, duration, currentTime, playing, transcription, container;

	function scrollToBottom() {
		if (container) {
			container.scrollTop = container.scrollHeight;
		}
	}

	function start() {
		audio.play();
		playing = true;
	}

	onMount(() => {
		duration = 146;
		currentTime = 0;

		audio = new Audio(`${base}/interview.mp3`);
		audio.addEventListener("loadeddata", () => {
			loaded = true;
			spoken.forEach((line) => {
				line.visible = false;
			});
		});

		audio.addEventListener("timeupdate", () => {
			duration = audio.duration;
			currentTime = audio.currentTime;

			let updatedSpoken = spoken.map((line) => {
				if (audio.currentTime >= line.time) {
					return { ...line, visible: true };
				} else {
					return { ...line, visible: false };
				}
			});

			transcription = updatedSpoken;

			scrollToBottom();
		});

		audio.addEventListener("ended", () => {
			playing = false;
		});
	});
</script>

<section id="interview" class="min-vh-100 d-flex flex-column justify-content-between py-5 text-light">
	<div class="container">
		<h1 class="text-center">"...but we didn't do anything."</h1>
		<h4 class="text-center fw-normal mb-5">A post concert discussion about algorithms and differing perspectives on music engagement as a performer/participant</h4>
	</div>
	<div class="container">
		<p>In this excerpt from our post-concert discussion, I try my best to isolate parts of the conversation that are relevant to the point of the analysis below. Bear in mind there is some cross chatter due to the recording being informal, but the transcription documents the content that'll be analyzed below.</p>
	</div>
	<div class="container d-flex justify-content-center align-items-start mb-2" style="height: 40px;">
		{#if !playing}
			<button class="btn btn-light" on:click="{start}">Play</button>
		{:else}
			<button class="btn btn-light" on:click="{() => (audio.pause(), (playing = false))}">Pause</button>
		{/if}
	</div>
	<div class="container mb-5" style="height: 400px;">
		{#if !loaded}
			<div class="spinner-border" role="status">
				<span class="visually-hidden">Loading...</span>
			</div>
		{:else if duration && currentTime}
			<input type="range" class="form-range" min="0" max="{duration}" bind:value="{currentTime}" on:input="{() => (audio.currentTime = currentTime)}" />
			<div class="d-flex flex-column gap-4" transition:fade>
				<div bind:this="{container}" class="d-flex flex-column gap-2 overflow-y-auto" style="height: 400px;">
					{#each transcription as { speaker, words, visible }}
						{#if visible}
							<div class="text-light pe-3" transition:fade>
								<p class="m-0">{speaker}</p>
								<p class="m-0">"{words}"</p>
							</div>
						{/if}
					{/each}
				</div>
			</div>
		{/if}
	</div>
	<div class="container mb-5">
		<h4>Responses to individual remarks</h4>
		<div class="mb-3">
			<figure class="mb-0 ms-3">
				<blockquote class="blockquote">
					<p>... we didn't do anything. 😂</p>
				</blockquote>
				<figcaption class="blockquote-footer mb-0">Vidula</figcaption>
			</figure>
			<p>You can read (a lot 😬) more about my stance on this sentiment below, but the tl;dr is that <span class="fw-bold">I believe we did</span>.</p>
		</div>
		<div class="mb-3">
			<figure class="mb-0 ms-3">
				<blockquote class="blockquote">
					<p>...just last week, we were... talking about alienation. This feels [like] alienation.</p>
				</blockquote>
				<figcaption class="blockquote-footer mb-0">Vidula</figcaption>
			</figure>
			<p>
				I can only really respond to this in the context of my own music. I interpret this two ways. First, I understand that engaging with something unfamiliar can be alienating. I've felt this many times in my own life, both in musical and non-musical situations. Second, in my music in particular, I focus
				primarily on accessibility. I want the music to do the <span class="fw-bold">opposite</span> of alienate. I want people regardless of their musical experience to make music by providing them with a format that allows them to make meaningful contributions to the music. This isn't a quality I've encountered
				in any algorithmic composition. In fact, the technology and methods that go into making involved computer music is usually too gnarly for the average musically fascinated person to get behind. I hope that the alienation wasn't experienced because of malice. As long as I'm writing music, anyone with a willingness
				to participate will have a place in any performances/activities I am a part of.
			</p>
		</div>
		<div class="mb-3">
			<figure class="mb-0 ms-3">
				<blockquote class="blockquote">
					<p>when you're actually practicing music, or painting, you're actually... also shaping you as a musician?</p>
				</blockquote>
				<figcaption class="blockquote-footer mb-0">Vidula</figcaption>
			</figure>
			<p>
				I talk more about my stance on talent and performance below, but I'm honing in on the use of the word "actually" here. The point behind how I designed this piece was that there didn't need to be rehearsal to produce output. Whether or not needing to invest more time into learning a skill shapes you any
				more than instances that aren't a result of time investment is a reflection of one's personal relationship with whatever discipline is being discussed. I feel like I've been shaped as a musician mostly through listening both for pleasure and educational purposes.
			</p>
		</div>
		<div class="mb-3">
			<figure class="mb-0 ms-3">
				<blockquote class="blockquote">
					<p>when you're actually practicing music, or painting, you're actually... also shaping you as a musician?</p>
				</blockquote>
				<figcaption class="blockquote-footer mb-0">Vidula</figcaption>
			</figure>
			<p>
				I talk more about my stance on talent and performance below, but I'm honing in on the use of the word "actually" here. The point behind how I designed this piece was that there didn't need to be rehearsal to produce output. Whether or not needing to invest more time into learning a skill shapes you any
				more than instances that aren't a result of time investment is a reflection of one's personal relationship with whatever discipline is being discussed. I feel like I've been shaped as a musician mostly through listening both for pleasure and educational purposes. This is why I followed up by asking,
				"This doesn't shape you as a musician?" Like I mentioned before, I talk about notions of talent below.
			</p>
		</div>
		<div class="mb-3">
			<figure class="mb-0 ms-3">
				<blockquote class="blockquote">
					<p>why it's not necessarily that we're doing anything, the computer's a tool to help automate a process that we would otherwise be doing.</p>
				</blockquote>
				<figcaption class="blockquote-footer mb-0">Forrest</figcaption>
			</figure>
			<p>
				Side note, I feel vein 👿 quoting myself. I program computers to generate sound in a perpetual, non-linearly organized, fashion in the majority of my music. Is this endemic to algorithmic composition? No, but, algorithms are what manipulate the data that turns into sound. This wouldn't be possible
				without a computer. In fact, no aspects of this piece would be possible without a computer. The fact that the technology is available to leverage for musical purposes is one of the primary reasons why pieces like this can even by conceptualized.
			</p>
		</div>
		<div class="mb-3">
			<figure class="mb-0 ms-3">
				<blockquote class="blockquote">
					<p>Like, of course, AI can write everything for me, but... I'm not becoming a writer.</p>
				</blockquote>
				<figcaption class="blockquote-footer mb-0">Vidula</figcaption>
			</figure>
			<p>
				AI has been a topic that gets brought up in <span class="fw-bold">every single talk I've had about my music</span>. For starters, it's a tool that I am fascinated in using in my own work; if for any reason just to implement it in a way that demonstrates that it's not a replacement for human creativity,
				but rather another tool (like a computer 💻). When someone sees something automatically pop up on their computer without having to put forth any effort, there's a general assumption that AI is being involved, but in fact, the piece was just programmed to run automatically with limited user engagement.
				AI is powered by algorithms that handle sets of source material that are then parsed in order to return a result the user asks for. ChatGPT using countless freely available publications. AI can seemingly generate new content, but it is a byproduct of aggregating human sources.
				<span class="fw-bold">The computer is not sentient.</span> Morality surrounding current AI art models is a completely separate discussion.
			</p>
		</div>
	</div>
	<div class="container">
		<h4>My thoughts on what constitutes performance/talent</h4>
		<p>
			What does it take in order to be considered as a performer? I believe a common sentiment is that performance art is a skill, regardless of discipline. The act of putting on a performance at any level of experience takes courage, vision, and planning. Musicians often spend months and years attempting to
			perfect the material they plan to put on a sonic display of their <span class="fw-bold">talent</span>. In this case, talent is understood as being more skilled in a particular area than the average person, which then leads to leveraging that skill to create an engaging and entertaining experience for an
			audience.
		</p>
		<p>
			However, the use of digital technology in music performance introduces an element of mystery when it comes to identifying what talent(s) are being put on display. For example, <a href="https://www.youtube.com/shorts/bkxi5RA3sPM" target="_blank">what the fuck do DJs actually do?</a>. The DJ spends countless
			hours blending, remixing, and choreographing their sets to present an experience that many don't appreciate. "I have a banger playlist on my Spotify... maybe I should talk to the club owner about booking my set." A bit tongue-and-cheek 😛, but conversations ike this have happened. Even though computers are
			multi-faceted instruments that have a steep learning curve, when used in a musical context, it's not anything in comparison to <a href="https://youtu.be/2l-3VhZ2Yik?si=uldd8nF9gzA-nlD8&t=165">6 year old Anna Lee playing a Paganini Violin Concerto</a>. Perhaps it's ignorance, or the fact that humans
			associate talent with some type of physical effort/visual output...
		</p>
		<p>
			I am of the opinion that <span class="fw-bold">talent and musicking are not mutually exclusive</span>. If a musically-fascinated person comes to a performance of my work, and there is an interactive component that blurs the line between audience member and ensemble member, even if their role was to press a
			button that pops up on their phone screen (which one might argue required little talent, if any), did that make their contribution any less meaningful? In the context of my aesthetic, the answer is no. The work isn't experienced completely without all aspects of the soundworld being present, no matter how
			big or small.
		</p>
		<p>
			A related aside, I have been teaching music (mainly guitar 🎸) since 2013, and I have a somewhat unique position of having a mostly adult student base, averaging 32 years of age. During the first lesson, which I call a consultation when first scheduling the meeting, one of the first questions I always ask
			is: why do you want to start playing the guitar? The answers are <span class="fw-bold">always variations of: I've always felt behind because I didn't start when I was younger.</span> Something I've only recently reflected on after hearing this sentiment is that it doesn't directly answer the question. It's what's
			inferred from the statement that answers the question. Starting as a child is the supposed best time to begin learning a skill, and that adult beginners are somehow lesser-than. Musical talent takes time, and we all know how elusive time is as we get older... But why does one need to be talented in order to
			playing meaningful music? Why do so many hold the opinion that the only music worth revering is music that is a product of talent?
		</p>
		<p>
			What even is talent? I touched on the theory I have about talent needing to be physical in some way, either by displaying some amount of (sometimes well) above average ability that can be measured visually, or something type of output that can have its value assessed immediately. Otherwise, talent can
			realistically only be appreciated by the ones who know about the act/output they're seeing intimately. In a musical context, we commonly evaluate talent by feats of mechanical skill: virtuosity, accuracy, improvisation, etc. Or, by the ability to have a profound emotional impact on the listener through
			careful selection of pitches, timbres, and rhythms. But why is talent the only thing that's worth celebrating in music? Can't we carry the same sentiment that a parent might have for their child at their first piano recital? Is the performance the most objectively moving concert that's ever been played?
			Probably not. But the parent is proud of the effort that's gone into the process.
		</p>
		<p>
			I'm sharing this not-so-scientific anecdote from my experience in the field as a private music teacher, which I'm sure other music instructors can relate with, to show that we've collectively as a society have married the ideas of talent and art. I am of the line of thinking that can be described in two
			ways. Either:
		</p>
		<ol>
			<li>Musicking doesn't require talent, period.</li>
			<li>People are more musical than they give themselves credit for, suggesting that the talent needed to be a valued musician isn't as great as we're led to believe.</li>
		</ol>
		<p>
			In the performance of this piece, I programmed the musical content to generate and play once the participant pressed a button. That's it. No other engagement needed. You could leave the room, browse your phone, or otherwise not be mentally present, and the music would continue to play. All of the algorithms
			that are crunching numbers will take care of the hard part. The algorithms are the real talented entities here, right? Did the participants actually do <span class="fw-bold">anything</span>? Yes, by being present, using your device(s) to participate in the piece, and consequently sending me your analysis
			file, you, objectively, did at least 3 things, none of which required talent in the way we traditionally define the term. Modes of experience vary in terms of the level of engagement that's expected of the participant. In the case of this piece, all that's expected of the participant is their presence with
			an internet enabled device. There is no hierarchy of engagement, everyone is talented, or no one is talented (depending on your relationship with the term).
		</p>
		<p>
			How is this at all related to algorithms or copyright at all? Well, not to get too deep and reiterate what I discuss in the <a href="#about">About</a> section, as far as algorithms are concerned, we don't see them. They are methods of manipulating data, and in my case as the composer, because the nature of the
			music is live content generation, we don't really attribute talent to output. We take the computer and the composer for granted if all we have the judge the piece is the output. Technology makes music accessible. The piece could've been as involved or as uninvolved as I willed it to be. I could've made it a
			choral piece where the algorithm spat out staff notation, and had a group of talented vocalists sing the melodies. I could've made the piece as uninvolved as possible by removing all buttons, just starting as soon as the webpage loaded. Context matters, and in the interest in the potential of bringing as many
			people into the fold as possible, I chose a more accessible route.
		</p>
		<p>
			Does this at all pertain to copyright? Mostly, no. I suppose a jury of peers that happens to not see the nuance in the algorithms that go into the music generation could see a piece like this as a low-effort attempt to leverage pre-existing music that has significantly more cultural currency than the piece
			itself. But, as the <a href="#About">About</a> section pretty much concluded, Fair Use is probably what would be employed legally in a hypothetical case like the one I've spun up for the sake of research.
		</p>
		<p>
			In summary, people have different relationships with talent and how it adds meaning to art, in particular. To my non-musical peers that I've talked with about music: taste and appreciation are entirely separate. Using a utility that provides musical shortcuts, or provides elements that are ready-to-use
			isn't something that should detract from our ability to appreciate the music. And, if all you need to do in order to partake in a music-making scenario is hitting a button, your contributions are both valued and necessary.
		</p>
	</div>
</section>

<style>
	.container {
		max-width: 800px;
	}

	.fw-bold {
		color: rgb(248, 203, 145);
	}

	a {
		color: rgb(145, 248, 214);
	}
</style>
