<script>
	import '$lib/styles/music-player.css';

	import { onMount } from 'svelte';
	import { APP_NAME } from '$lib/js/constants';

	const imageModules = import.meta.glob('$lib/assets/images/projects/music-player/*.jpg', {
		eager: true,
		import: 'default'
	});

	const imagePath = Object.keys(imageModules)[0].split('/').slice(0, -1).join('/');

	const audioModules = import.meta.glob('$lib/assets/audios/projects/music-player/*.mp3', {
		eager: true,
		import: 'default'
	});

	const audioPath = Object.keys(audioModules)[0].split('/').slice(0, -1).join('/');

	/**
	 * @type {HTMLAudioElement}
	 */
	let music;
	let isPlaying = false;
	let playCssClass = 'fa-play';
	let playBtnTitle = 'Play';
	/**
	 * @type {HTMLHeadingElement}
	 */
	let title;
	/**
	 * @type {HTMLHeadingElement}
	 */
	let artist;
	/**
	 * @type {HTMLImageElement}
	 */
	let image;
	/**
	 * @type {HTMLDivElement}
	 */
	let progress;
	/**
	 * @type {HTMLDivElement}
	 */
	let progressContainer;
	let songIndex = 0;
	/**
	 * @type {HTMLSpanElement}
	 */
	let currentTimeEl;
	/**
	 * @type {HTMLSpanElement}
	 */
	let durationEl;

	const songs = [
		{
			name: 'jacinto-1',
			displayName: 'Electric Chill Machine',
			artist: 'Jacinto Design'
		},
		{
			name: 'jacinto-2',
			displayName: 'Seven Nation Army (Remix)',
			artist: 'Jacinto Design'
		},
		{
			name: 'jacinto-3',
			displayName: 'Goodnight, Disco Queen',
			artist: 'Jacinto Design'
		},
		{
			name: 'metric-1',
			displayName: 'Front Row (Remix)',
			artist: 'Metric/Jacinto Design'
		}
	];

	function playSong() {
		isPlaying = true;
		music.play();
		playCssClass = 'fa-pause';
		playBtnTitle = 'Pause';
	}

	function pauseSong() {
		isPlaying = false;
		music.pause();
		playCssClass = 'fa-play';
		playBtnTitle = 'Play';
	}

	/**
	 * @param {{ displayName: string ; artist: string ; name: string; }} song
	 */
	function loadSong(song) {
		title.textContent = song.displayName;
		artist.textContent = song.artist;
		// @ts-ignore
		music.src = audioModules[`${audioPath}/${song.name}.mp3`];
		// @ts-ignore
		image.src = imageModules[`${imagePath}/${song.name}.jpg`];
	}

	function nextSong() {
		songIndex++;
		if (songIndex >= songs.length) {
			songIndex = 0;
		}
		loadSong(songs[songIndex]);
		if (isPlaying) {
			playSong();
		}
	}

	function prevSong() {
		songIndex--;
		if (songIndex < 0) {
			songIndex = songs.length - 1;
		}
		loadSong(songs[songIndex]);
		if (isPlaying) {
			playSong();
		}
	}

	/**
	 * @param {Event} $event
	 */
	function updateProgressBar($event) {
		if (isPlaying) {
			// @ts-ignore
			const { duration, currentTime } = $event.target;
			const progressPercent = (currentTime / duration) * 100;
			progress.style.width = `${progressPercent}%`;

			const currentTimeMins = Math.round(currentTime / 60);
			const currentTimeSecs = Math.round(currentTime % 60);
			currentTimeEl.textContent = `${currentTimeMins}:${currentTimeSecs.toLocaleString(undefined, {
				minimumIntegerDigits: 2,
				useGrouping: false
			})}`;
		}
	}

	function initDisplay() {
		const durationMins = Math.round(music.duration / 60);
		const durationSecs = Math.round(music.duration % 60);
		durationEl.textContent = `${durationMins}:${durationSecs.toLocaleString(undefined, {
			minimumIntegerDigits: 2,
			useGrouping: false
		})}`;
		currentTimeEl.textContent = '0:00';
	}

	/**
	 * @param {Event} $event
	 * @this {HTMLDivElement}
	 */
	function setProgressBar($event) {
		const width = this.clientWidth;
		// @ts-ignore
		const clickX = $event.offsetX;
		const { duration } = music;
		music.currentTime = (clickX / width) * duration;
	}

	onMount(() => {
		loadSong(songs[songIndex]);
		music.preload = 'metadata';
		music.autoplay = false;
		music.addEventListener('timeupdate', updateProgressBar);
		music.addEventListener('loadedmetadata', initDisplay);
		progressContainer.addEventListener('click', setProgressBar);
		music.addEventListener('ended', nextSong);
	});
</script>

<svelte:head>
	<title>{APP_NAME} | Music Player</title>
</svelte:head>

<div class="player-container">
	<div class="img-container">
		<img alt="Album Art" bind:this={image} />
	</div>
	<!-- svelte-ignore a11y-missing-content -->
	<h2 id="title" bind:this={title} />
	<!-- svelte-ignore a11y-missing-content -->
	<h3 id="artist" bind:this={artist} />
	<audio bind:this={music} />
	<div class="progress-container" id="progress-container" bind:this={progressContainer}>
		<div class="progress" id="progress" bind:this={progress} />
		<div class="duration-wrapper">
			<span id="current-time" bind:this={currentTimeEl} />
			<span id="duration" bind:this={durationEl} />
		</div>
	</div>
	<div class="player-controls">
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<!-- svelte-ignore a11y-no-static-element-interactions -->
		<i class="fas fa-backward" id="prev" title="Previous" on:click={prevSong} />
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<!-- svelte-ignore a11y-no-static-element-interactions -->
		<i
			class="fas {playCssClass} main-button"
			id="play"
			title={playBtnTitle}
			on:click={() => (isPlaying ? pauseSong() : playSong())}
		/>
		<!-- svelte-ignore a11y-click-events-have-key-events -->
		<!-- svelte-ignore a11y-no-static-element-interactions -->
		<i class="fas fa-forward" id="next" title="Next" on:click={nextSong} />
	</div>
</div>

<style>
	.player-container {
		height: 500px;
		width: 400px;
		background-color: #e7e7e7;
		border-radius: 20px;
		box-shadow: 0 15px 30px 5px rgba(0, 0, 0, 0.3);
	}

	.img-container {
		height: 300px;
		width: 300px;
		position: relative;
		top: -50px;
		left: 50px;
	}

	.img-container img {
		height: 100%;
		width: 100%;
		object-fit: cover;
		border-radius: 20px;
		box-shadow: 0 5px 30px 5px rgba(0, 0, 0, 0.3);
	}

	h2 {
		font-size: 25px;
		text-align: center;
		margin: 0;
	}

	h3 {
		font-size: 20px;
		text-align: center;
		font-weight: 400;
		margin: 5px 0 0;
	}

	/* Progress */
	.progress-container {
		background: #fff;
		border-radius: 5px;
		cursor: pointer;
		margin: 40px 20px;
		height: 4px;
		width: 90%;
	}

	.progress {
		background: #242323;
		border-radius: 5px;
		height: 100%;
		/* change this to show progress */
		width: 0%;
		transition: width 0.1s linear;
	}

	.duration-wrapper {
		position: relative;
		top: -25px;
		display: flex;
		justify-content: space-between;
	}

	/* Controls */
	.player-controls {
		position: relative;
		top: -15px;
		left: 120px;
		width: 200px;
	}

	.fas {
		font-size: 30px;
		color: rgb(129, 129, 129);
		margin-right: 30px;
		cursor: pointer;
		user-select: none;
	}

	.fas:hover {
		filter: brightness(80%);
	}

	.main-button {
		font-size: 40px;
		position: relative;
		top: 3px;
	}

	/* Media Query: iPhone (Vertical) */
	@media screen and (max-width: 376px) {
		.player-container {
			width: 95vw;
		}

		.img-container {
			left: 29px;
		}

		h2 {
			font-size: 20px;
		}

		h3 {
			font-size: 15px;
		}

		.player-controls {
			top: -10px;
			left: 100px;
		}
	}
</style>
