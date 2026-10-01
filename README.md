
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>M3 - Modern Music Player</title>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jsmediatags/3.9.5/jsmediatags.min.js"></script>
    <style>
        :root {
            --bg-color: #121212;
            --primary-surface-color: #1d1d1d;
            --secondary-surface-color: #282828;
            --primary-text-color: #ffffff;
            --secondary-text-color: #b3b3b3;
            --accent-color: #1db954;
            --accent-color-hover: #1ed760;
            --shadow-color: rgba(0, 0, 0, 0.5);
            --scrollbar-thumb-color: #434343;
            --scrollbar-track-color: #1d1d1d;
        }

        body.light-mode {
            --bg-color: #f5f5f5;
            --primary-surface-color: #ffffff;
            --secondary-surface-color: #e0e0e0;
            --primary-text-color: #000000;
            --secondary-text-color: #5f5f5f;
            --accent-color: #1a73e8;
            --accent-color-hover: #2980fc;
            --shadow-color: rgba(0, 0, 0, 0.15);
            --scrollbar-thumb-color: #c1c1c1;
            --scrollbar-track-color: #f1f1f1;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }

        body {
            font-family: 'Inter', sans-serif;
            background-color: var(--bg-color);
            color: var(--primary-text-color);
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            overflow: hidden;
            transition: background-color 0.3s, color 0.3s;
        }

        .app-container {
            display: flex;
            width: 100%;
            max-width: 1200px;
            height: 100%;
            max-height: 800px;
            background-color: var(--primary-surface-color);
            border-radius: 12px;
            box-shadow: 0 10px 30px var(--shadow-color);
            overflow: hidden;
        }

        /* Player Section */
        .player {
            flex: 2;
            padding: 40px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            position: relative;
        }
        
        #theme-toggle {
            position: absolute;
            top: 20px;
            right: 20px;
            background: var(--secondary-surface-color);
            border: none;
            color: var(--primary-text-color);
            width: 40px;
            height: 40px;
            border-radius: 50%;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: background-color 0.2s;
        }
        
        #theme-toggle:hover {
            background: var(--accent-color);
        }
        
        .theme-icon {
             width: 20px;
             height: 20px;
        }

        .album-art-container {
            position: relative;
            width: 100%;
            max-width: 320px;
            margin-bottom: 30px;
        }

        #album-art {
            width: 100%;
            aspect-ratio: 1 / 1;
            border-radius: 12px;
            box-shadow: 0 8px 24px var(--shadow-color);
            object-fit: cover;
            transition: transform 0.3s ease-in-out;
        }

        #visualizer {
            position: absolute;
            bottom: 5px;
            left: 5px;
            right: 5px;
            height: 80px;
            opacity: 0.6;
            border-bottom-left-radius: 10px;
            border-bottom-right-radius: 10px;
        }

        .track-info {
            text-align: center;
            margin-bottom: 30px;
        }

        #track-title {
            font-size: 24px;
            font-weight: 700;
            margin-bottom: 8px;
        }

        #track-artist {
            font-size: 16px;
            color: var(--secondary-text-color);
        }

        .progress-container {
            width: 100%;
            max-width: 400px;
            margin-bottom: 20px;
        }

        #progress {
            width: 100%;
            -webkit-appearance: none;
            appearance: none;
            height: 6px;
            background: var(--secondary-surface-color);
            border-radius: 3px;
            outline: none;
            cursor: pointer;
        }

        #progress::-webkit-slider-thumb {
            -webkit-appearance: none;
            appearance: none;
            width: 16px;
            height: 16px;
            background: var(--accent-color);
            border-radius: 50%;
            cursor: pointer;
        }

        #progress::-moz-range-thumb {
            width: 16px;
            height: 16px;
            background: var(--accent-color);
            border-radius: 50%;
            cursor: pointer;
        }

        .time-stamps {
            display: flex;
            justify-content: space-between;
            font-size: 12px;
            color: var(--secondary-text-color);
            margin-top: 8px;
        }

        .controls {
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 20px;
        }

        .control-btn {
            background: none;
            border: none;
            color: var(--secondary-text-color);
            cursor: pointer;
            transition: color 0.2s, transform 0.1s;
        }

        .control-btn:hover {
            color: var(--primary-text-color);
        }
        
        .control-btn.active {
            color: var(--accent-color);
        }

        #play-pause-btn {
            background-color: var(--accent-color);
            color: white;
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            box-shadow: 0 4px 10px rgba(0,0,0,0.3);
        }
        #play-pause-btn:hover {
             background-color: var(--accent-color-hover);
             transform: scale(1.05);
        }

        /* Playlist Section */
        .playlist {
            flex: 1;
            background-color: var(--secondary-surface-color);
            padding: 20px;
            overflow-y: auto;
            display: flex;
            flex-direction: column;
        }
        
        .playlist-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }

        #add-music-btn {
            background-color: var(--accent-color);
            color: white;
            border: none;
            width: 32px;
            height: 32px;
            border-radius: 50%;
            font-size: 24px;
            font-weight: bold;
            line-height: 32px;
            text-align: center;
            cursor: pointer;
            transition: background-color 0.2s, transform 0.1s;
        }
        #add-music-btn:hover {
             background-color: var(--accent-color-hover);
             transform: scale(1.1);
        }

        #playlist-ul {
            list-style: none;
        }

        .playlist-item {
            display: flex;
            align-items: center;
            padding: 12px;
            border-radius: 8px;
            cursor: pointer;
            transition: background-color 0.2s;
        }

        .playlist-item:hover {
            background-color: rgba(255, 255, 255, 0.05);
        }
        
        .playlist-item.active {
            background-color: var(--accent-color);
            color: white;
        }
        
        .playlist-item.active .playlist-artist {
            color: rgba(255, 255, 255, 0.8);
        }

        .playlist-artwork {
            width: 40px;
            height: 40px;
            border-radius: 4px;
            margin-right: 15px;
            object-fit: cover;
        }

        .playlist-info .playlist-title {
            font-weight: 600;
        }

        .playlist-info .playlist-artist {
            font-size: 14px;
            color: var(--secondary-text-color);
        }

        /* Scrollbar */
        .playlist::-webkit-scrollbar {
            width: 8px;
        }
        .playlist::-webkit-scrollbar-track {
            background: var(--scrollbar-track-color);
        }
        .playlist::-webkit-scrollbar-thumb {
            background: var(--scrollbar-thumb-color);
            border-radius: 4px;
        }

        /* Responsive Design */
        @media (max-width: 768px) {
            body {
                height: auto;
                padding: 10px;
            }
            .app-container {
                flex-direction: column;
                height: 95vh;
                max-height: none;
            }
            .player {
                flex: 3;
                padding: 20px;
                order: 2;
            }
            .playlist {
                flex: 2;
                order: 1;
            }
            .album-art-container {
                max-width: 200px;
            }
             #track-title { font-size: 20px; }
            #track-artist { font-size: 14px; }
        }
    </style>
<link rel="stylesheet" href="/index.css">
</head>
<body>
    <div class="app-container">
        <main class="player">
            <button id="theme-toggle">
                <!-- SVG icons will be injected by JS -->
            </button>
            <div class="album-art-container">
                <img src="https://picsum.photos/seed/placeholder/500/500" alt="Album Art" id="album-art">
                <canvas id="visualizer"></canvas>
            </div>
            <div class="track-info">
                <h2 id="track-title">Select a Song</h2>
                <p id="track-artist">Modern Music Player</p>
            </div>
            <div class="progress-container">
                <input type="range" id="progress" value="0" step="1">
                <div class="time-stamps">
                    <span id="current-time">0:00</span>
                    <span id="duration">0:00</span>
                </div>
            </div>
            <div class="controls">
                <button class="control-btn" id="shuffle-btn">
                    <svg class="icon" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="16 3 21 3 21 8"></polyline><line x1="4" y1="20" x2="21" y2="3"></line><polyline points="16 16 21 16 21 21"></polyline><line x1="15" y1="15" x2="21" y2="21"></line><line x1="4" y1="4" x2="11" y2="11"></line></svg>
                </button>
                <button class="control-btn" id="prev-btn">
                     <svg class="icon" width="28" height="28" viewBox="0 0 24 24" fill="currentColor"><path d="M6 6h2v12H6zm3.5 6l8.5 6V6z"></path></svg>
                </button>
                <button class="control-btn" id="play-pause-btn">
                    <!-- Play/Pause icon here -->
                </button>
                <button class="control-btn" id="next-btn">
                     <svg class="icon" width="28" height="28" viewBox="0 0 24 24" fill="currentColor"><path d="M6 18l8.5-6L6 6v12zM16 6v12h2V6h-2z"></path></svg>
                </button>
                <button class="control-btn" id="repeat-btn">
                    <svg class="icon" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="17 1 21 5 17 9"></polyline><path d="M3 11V9a4 4 0 0 1 4-4h14"></path><polyline points="7 23 3 19 7 15"></polyline><path d="M21 13v2a4 4 0 0 1-4 4H3"></path></svg>
                </button>
            </div>
            <audio id="audio" crossOrigin="anonymous"></audio>
        </main>
        <aside class="playlist">
            <div class="playlist-header">
                <h2>Playlist</h2>
                <button id="add-music-btn" title="Add Music">+</button>
            </div>
            <input type="file" id="file-input" multiple accept="audio/*" style="display: none;">
            <ul id="playlist-ul">
                <!-- Playlist items will be injected by JS -->
            </ul>
        </aside>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // --- DOM ELEMENTS ---
            const audio = document.getElementById('audio');
            const albumArt = document.getElementById('album-art');
            const trackTitle = document.getElementById('track-title');
            const trackArtist = document.getElementById('track-artist');
            const progress = document.getElementById('progress');
            const currentTimeEl = document.getElementById('current-time');
            const durationEl = document.getElementById('duration');
            const playPauseBtn = document.getElementById('play-pause-btn');
            const prevBtn = document.getElementById('prev-btn');
            const nextBtn = document.getElementById('next-btn');
            const shuffleBtn = document.getElementById('shuffle-btn');
            const repeatBtn = document.getElementById('repeat-btn');
            const playlistUl = document.getElementById('playlist-ul');
            const themeToggle = document.getElementById('theme-toggle');
            const visualizerCanvas = document.getElementById('visualizer');
            const addMusicBtn = document.getElementById('add-music-btn');
            const fileInput = document.getElementById('file-input');

            // --- ICONS ---
            const playIcon = `<svg class="icon" width="32" height="32" viewBox="0 0 24 24" fill="currentColor"><path d="M8 5v14l11-7z"></path></svg>`;
            const pauseIcon = `<svg class="icon" width="32" height="32" viewBox="0 0 24 24" fill="currentColor"><path d="M6 19h4V5H6v14zm8-14v14h4V5h-4z"></path></svg>`;
            const sunIcon = `<svg class="theme-icon" viewBox="0 0 24 24" fill="currentColor"><circle cx="12" cy="12" r="5"></circle><line x1="12" y1="1" x2="12" y2="3"></line><line x1="12" y1="21" x2="12" y2="23"></line><line x1="4.22" y1="4.22" x2="5.64" y2="5.64"></line><line x1="18.36" y1="18.36" x2="19.78" y2="19.78"></line><line x1="1" y1="12" x2="3" y2="12"></line><line x1="21" y1="12" x2="23" y2="12"></line><line x1="4.22" y1="19.78" x2="5.64" y2="18.36"></line><line x1="18.36" y1="5.64" x2="19.78" y2="4.22"></line></svg>`;
            const moonIcon = `<svg class="theme-icon" viewBox="0 0 24 24" fill="currentColor"><path d="M21 12.79A9 9 0 1 1 11.21 3 7 7 0 0 0 21 12.79z"></path></svg>`;

            // --- AUDIO VISUALIZER ---
            let audioContext, analyser, source, animationFrameId;
            const canvasCtx = visualizerCanvas.getContext('2d');

            function setupAudioContext() {
                if (audioContext) return;
                try {
                    audioContext = new (window.AudioContext || window.webkitAudioContext)();
                    analyser = audioContext.createAnalyser();
                    source = audioContext.createMediaElementSource(audio);
                    source.connect(analyser);
                    analyser.connect(audioContext.destination);
                    analyser.fftSize = 128;
                } catch (e) {
                    console.error("Web Audio API is not supported in this browser", e);
                }
            }
            
            function drawVisualizer() {
                if (!analyser) {
                    return;
                }
                const bufferLength = analyser.frequencyBinCount;
                const dataArray = new Uint8Array(bufferLength);
                analyser.getByteFrequencyData(dataArray);

                canvasCtx.clearRect(0, 0, visualizerCanvas.width, visualizerCanvas.height);

                const barWidth = (visualizerCanvas.width / bufferLength) * 1.5;
                let x = 0;
                const accentColor = getComputedStyle(document.documentElement).getPropertyValue('--accent-color').trim();

                for (let i = 0; i < bufferLength; i++) {
                    const barHeight = dataArray[i] / 2;
                    canvasCtx.fillStyle = accentColor;
                    canvasCtx.fillRect(x, visualizerCanvas.height - barHeight, barWidth, barHeight);
                    x += barWidth + 2;
                }

                animationFrameId = requestAnimationFrame(drawVisualizer);
            }

            // --- STATE ---
            const playlist = [
                { title: "Lofi Chill", artist: "BoDleasons", url: "https://assets.mixkit.co/music/preview/mixkit-lofi-chill-121.mp3", artwork: "https://picsum.photos/seed/lofi/500/500" },
                { title: "Sleepy Cat", artist: "Alejandro Magaña", url: "https://assets.mixkit.co/music/preview/mixkit-sleepy-cat-135.mp3", artwork: "https://picsum.photos/seed/cat/500/500" },
                { title: "A Very Happy Christmas", artist: "All Good Folks", url: "https://assets.mixkit.co/music/preview/mixkit-a-very-happy-christmas-897.mp3", artwork: "https://picsum.photos/seed/christmas/500/500" },
                { title: "Comical", artist: "Ahjay Stelino", url: "https://assets.mixkit.co/music/preview/mixkit-comical-2.mp3", artwork: "https://picsum.photos/seed/comical/500/500" },
                { title: "Just Kidding", artist: "Sunsearcher", url: "https://assets.mixkit.co/music/preview/mixkit-just-kidding-35.mp3", artwork: "https://picsum.photos/seed/kidding/500/500" },
                { title: "Hollidays", artist: "Grigoriy Nuzhny", url: "https://assets.mixkit.co/music/preview/mixkit-hollidays-690.mp3", artwork: "https://picsum.photos/seed/hollidays/500/500" }
            ];

            let currentTrackIndex = 0;
            let isPlaying = false;
            let isShuffle = false;
            let repeatMode = 'off'; // 'off', 'all', 'one'

            // --- FUNCTIONS ---
            function loadTrack(index) {
                if (index < 0 || index >= playlist.length) return;
                const track = playlist[index];
                trackTitle.textContent = track.title;
                trackArtist.textContent = track.artist;
                albumArt.src = track.artwork;
                audio.src = track.url;
                currentTrackIndex = index;
                updateActivePlaylistItem();
            }

            function playSong() {
                setupAudioContext();
                isPlaying = true;
                playPauseBtn.innerHTML = pauseIcon;
                audio.play();
                albumArt.style.transform = "scale(1.05)";
                cancelAnimationFrame(animationFrameId);
                drawVisualizer();
            }

            function pauseSong() {
                isPlaying = false;
                playPauseBtn.innerHTML = playIcon;
                audio.pause();
                albumArt.style.transform = "scale(1)";
                cancelAnimationFrame(animationFrameId);
            }
            
            function togglePlayPause() {
                if (isPlaying) {
                    pauseSong();
                } else {
                    playSong();
                }
            }

            function prevTrack() {
                if (isShuffle) {
                    playRandomTrack();
                } else {
                    currentTrackIndex = (currentTrackIndex - 1 + playlist.length) % playlist.length;
                    loadTrack(currentTrackIndex);
                    playSong();
                }
            }

            function nextTrack() {
                if (isShuffle) {
                    playRandomTrack();
                } else {
                    currentTrackIndex = (currentTrackIndex + 1) % playlist.length;
                    loadTrack(currentTrackIndex);
                    playSong();
                }
            }
            
            function playRandomTrack() {
                let newIndex;
                do {
                    newIndex = Math.floor(Math.random() * playlist.length);
                } while (playlist.length > 1 && newIndex === currentTrackIndex);
                currentTrackIndex = newIndex;
                loadTrack(currentTrackIndex);
                playSong();
            }

            function updateProgress() {
                const { duration, currentTime } = audio;
                if (duration) {
                    const progressPercent = (currentTime / duration) * 100;
                    progress.value = currentTime;
                    currentTimeEl.textContent = formatTime(currentTime);
                }
            }
            
            function setProgressFromInput() {
                if (audio.duration) {
                    audio.currentTime = progress.value;
                }
            }

            function handleTrackEnd() {
                if (repeatMode === 'one') {
                    audio.currentTime = 0;
                    playSong();
                } else if (repeatMode === 'all') {
                    nextTrack();
                } else { // 'off'
                    if (currentTrackIndex === playlist.length - 1) {
                        pauseSong();
                    } else {
                        nextTrack();
                    }
                }
            }

            function toggleShuffle() {
                isShuffle = !isShuffle;
                shuffleBtn.classList.toggle('active', isShuffle);
            }

            function toggleRepeat() {
                const repeatIconSvg = `<svg class="icon" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="17 1 21 5 17 9"></polyline><path d="M3 11V9a4 4 0 0 1 4-4h14"></path><polyline points="7 23 3 19 7 15"></polyline><path d="M21 13v2a4 4 0 0 1-4 4H3"></path></svg>`;
                if (repeatMode === 'off') {
                    repeatMode = 'all';
                    repeatBtn.classList.add('active');
                    repeatBtn.innerHTML = repeatIconSvg;
                } else if (repeatMode === 'all') {
                    repeatMode = 'one';
                    repeatBtn.innerHTML = `<svg class="icon" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><polyline points="17 1 21 5 17 9"></polyline><path d="M3 11V9a4 4 0 0 1 4-4h14"></path><polyline points="7 23 3 19 7 15"></polyline><path d="M21 13v2a4 4 0 0 1-4 4H3"></path><text x="11.5" y="14" font-size="9" font-weight="bold" fill="currentColor" text-anchor="middle">1</text></svg>`;
                } else {
                    repeatMode = 'off';
                    repeatBtn.classList.remove('active');
                    repeatBtn.innerHTML = repeatIconSvg;
                }
            }

            function formatTime(seconds) {
                const minutes = Math.floor(seconds / 60);
                const secs = Math.floor(seconds % 60);
                return `${minutes}:${secs < 10 ? '0' : ''}${secs}`;
            }
            
            function addTrackToPlaylistUI(track, index) {
                const li = document.createElement('li');
                li.classList.add('playlist-item');
                li.dataset.index = index;
                li.innerHTML = `
                    <img src="${track.artwork}" alt="${track.title}" class="playlist-artwork">
                    <div class="playlist-info">
                        <span class="playlist-title">${track.title}</span>
                        <span class="playlist-artist">${track.artist}</span>
                    </div>
                `;
                li.addEventListener('click', () => {
                    loadTrack(index);
                    playSong();
                });
                playlistUl.appendChild(li);
            }

            function createPlaylist() {
                playlist.forEach(addTrackToPlaylistUI);
            }

            function updateActivePlaylistItem() {
                document.querySelectorAll('.playlist-item').forEach(item => {
                    item.classList.remove('active');
                });
                const activeItem = document.querySelector(`.playlist-item[data-index="${currentTrackIndex}"]`);
                if (activeItem) {
                    activeItem.classList.add('active');
                    activeItem.scrollIntoView({ behavior: 'smooth', block: 'nearest' });
                }
            }

            // --- Theme Toggle ---
            function applyTheme(theme) {
                if (theme === 'light') {
                    document.body.classList.add('light-mode');
                    themeToggle.innerHTML = moonIcon;
                } else {
                    document.body.classList.remove('light-mode');
                    themeToggle.innerHTML = sunIcon;
                }
            }

            function toggleTheme() {
                const currentTheme = document.body.classList.contains('light-mode') ? 'light' : 'dark';
                const newTheme = currentTheme === 'light' ? 'dark' : 'light';
                localStorage.setItem('theme', newTheme);
                applyTheme(newTheme);
            }
            
            // --- Add Music Functionality ---
            function handleFileSelect(event) {
                const files = event.target.files;
                if (!files.length) return;

                for (const file of files) {
                    processFile(file);
                }
                // Clear the input value to allow selecting the same file again
                fileInput.value = '';
            }

            function processFile(file) {
                 window.jsmediatags.read(file, {
                    onSuccess: (tag) => {
                        const tags = tag.tags;
                        const artwork = getArtworkUrl(tags.picture);
                        const newTrack = {
                            title: tags.title || file.name.replace(/\.[^/.]+$/, ""),
                            artist: tags.artist || "Unknown Artist",
                            url: URL.createObjectURL(file),
                            artwork: artwork
                        };
                        playlist.push(newTrack);
                        addTrackToPlaylistUI(newTrack, playlist.length - 1);
                    },
                    onError: (error) => {
                        console.warn('Error reading media tags:', error);
                        // Fallback for files with no tags or read errors
                        const newTrack = {
                            title: file.name.replace(/\.[^/.]+$/, ""),
                            artist: "Unknown Artist",
                            url: URL.createObjectURL(file),
                            artwork: `https://picsum.photos/seed/${file.name}/100/100`
                        };
                        playlist.push(newTrack);
                        addTrackToPlaylistUI(newTrack, playlist.length - 1);
                    }
                });
            }

            function getArtworkUrl(picture) {
                if (!picture) {
                    const seed = Math.random().toString(36).substring(7);
                    return `https://picsum.photos/seed/${seed}/100/100`;
                }
                const { data, format } = picture;
                let base64String = "";
                for (let i = 0; i < data.length; i++) {
                    base64String += String.fromCharCode(data[i]);
                }
                return `data:${format};base64,${window.btoa(base64String)}`;
            }


            // --- EVENT LISTENERS ---
            playPauseBtn.addEventListener('click', togglePlayPause);
            prevBtn.addEventListener('click', prevTrack);
            nextBtn.addEventListener('click', nextTrack);
            audio.addEventListener('timeupdate', updateProgress);
            audio.addEventListener('loadedmetadata', () => {
                durationEl.textContent = formatTime(audio.duration);
                progress.max = audio.duration;
            });
            audio.addEventListener('ended', handleTrackEnd);
            progress.addEventListener('input', setProgressFromInput);
            shuffleBtn.addEventListener('click', toggleShuffle);
            repeatBtn.addEventListener('click', toggleRepeat);
            themeToggle.addEventListener('click', toggleTheme);
            addMusicBtn.addEventListener('click', () => fileInput.click());
            fileInput.addEventListener('change', handleFileSelect);

            // --- INITIALIZATION ---
            function init() {
                playPauseBtn.innerHTML = playIcon;
                createPlaylist();
                loadTrack(0);
                const savedTheme = localStorage.getItem('theme') || 'dark';
                applyTheme(savedTheme);
            }

            init();
        });
    </script>
<script type="module" src="/index.tsx"></script>
</body>
</html>
