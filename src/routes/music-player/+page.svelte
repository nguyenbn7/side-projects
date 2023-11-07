<script>
    import './styles.css';
</script>
<svelte:head>
	<title>Music Player</title>
	<link
		rel="stylesheet"
		href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.10.2/css/all.min.css"
	/>
</svelte:head>

<div class="player-container">
	<div class="img-container">
		<img src="music-player/img/jacinto-1.jpg" alt="Album Art" />
	</div>
	<h2 id="title">Electric Chill Machine</h2>
	<h3 id="artist">Jacinto</h3>
	<audio src="music-player/music/jacinto-1.mp3" controls />
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
		width: 66%;
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
