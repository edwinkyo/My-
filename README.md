<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My Image, Video Editor, and MP3 Player</title>
    <style>
        /* General body and background styles */
        html, body {
            background: #f2f2f2;
            background: -moz-radial-gradient(center, ellipse cover,  #ffffff 0%, #ffffff 26%, #f5f5f5 59%, #f5f5f5 77%, #cecece 100%);
            background: -webkit-gradient(radial, center center, 0, center center, 100%, color-stop(0%,#ffffff), color-stop(26%,#ffffff), color-stop(59%,#f5f5f5), color-stop(77%,#f5f5f5), color-stop(100%,#cecece));
            background: -webkit-radial-gradient(center, ellipse cover,  #ffffff 0%,#ffffff 26%,#f5f5f5 59%,#f5f5f5 77%,#cecece 100%);
            background: -o-radial-gradient(center, ellipse cover,  #ffffff 0%,#ffffff 26%,#f5f5f5 59%,#f5f5f5 77%,#cecece 100%);
            background: -ms-radial-gradient(center, ellipse cover,  #ffffff 0%,#ffffff 26%,#f5f5f5 59%,#f5f5f5 77%,#cecece 100%);
            background: radial-gradient(ellipse at center,  #ffffff 0%,#ffffff 26%,#f5f5f5 59%,#f5f5f5 77%,#cecece 100%);
            overflow: hidden;
            padding: 0;
            margin: 0;
            height: 100%;
        }

        /* Slideshow styles */
        #slideshowContainer { 
            width: 80%; 
            margin: auto; 
            text-align: center; 
            position: relative; 
        }
        #slideImage { 
            width: 100%; 
            height: auto; 
            transition: opacity 1s ease-in-out; 
        }

        /* Header styles */
        header { 
            background-color: #007BFF; 
            color: white; 
            text-align: center; 
            padding: 1rem; 
        }

        /* Main content styles */
        main { 
            padding: 1rem; 
        }

        /* Footer styles */
        footer { 
            text-align: center; 
            margin-top: 20px; 
            padding: 1rem; 
            background-color: #222; 
            color: white; 
        }

        /* Image canvas styles */
        #imageCanvas { 
            width: 80%; 
            margin: auto; 
            display: block; 
        }

        /* Form input styles */
        form { 
            margin-top: 20px; 
        }

        /* Text formatting */
        u {
            text-decoration: underline;
        }
    </style>
</head>
<body>
    <header>
        <h1>Welcome to My Image, Video Editor, and MP3 Player</h1>
    </header>

    <main>
        <h2>Upload Your Image or Video</h2>
        <form id="uploadForm">
            <label for="fileInput">Select Image or Video:</label>
            <input type="file" id="fileInput" accept="image/*,video/*" required>
            <br><br>
            <button type="submit">Upload</button>
        </form>
        
        <h3>Upload File Tambahan</h3>
        <form id="additionalUploadForm">
            <label for="additionalFileInput">Pilih File untuk Diunggah:</label>
            <input type="file" id="additionalFileInput" accept="image/*,video/*,document/*" required>
            <br><br>
            <button type="submit">Unggah</button>
        </form>

        <h3>Slide Show with Music</h3>
        <div id="slideshowContainer">
            <img id="slideImage" src="" alt="Slideshow Image">
        </div>
        <audio id="backgroundMusic" loop>
            <source src="your-music-file.mp3" type="audio/mp3">
            Your browser does not support the audio element.
        </audio>
        <button onclick="startSlideshow()">Start Slideshow</button>
        <button onclick="stopSlideshow()">Stop Slideshow</button>
        <button onclick="toggleMusic()">Toggle Music</button>

        <h3>Edit Image and Apply Filters</h3>
        <canvas id="imageCanvas"></canvas>
        <button onclick="applyFilter()">Apply Filter</button>

        <h3>Edit Video</h3>
        <video id="videoPlayer" controls></video>
        <button onclick="applyVideoEffect()">Apply Effect</button>

        <h3>Download Edited File</h3>
        <button onclick="downloadFile()">Download File</button>

        <h3>MP3 Player</h3>
        <audio controls>
            <source src="your-audio-file.mp3" type="audio/mp3">
            Your browser does not support the audio element.
        </audio>

        <div id="message"></div>

        <h4>Additional Content</h4>
        <ul>
            <li>This is the first point in the list.</li>
            <li>This is the second point in the list.</li>
            <li>This is the third point in the list.</li>
        </ul>
        <p>In this example, we added a <a href="https://www.example.com" target="_blank">link in the text</a> that can be clicked and directs to another page.</p>
        <p>This line has <strong>bold words with the strong tag</strong>.</p>
        <p>Meanwhile, the words in this line are <em>italicized with the em tag</em>.</p>
        <p>In this line, <u>there is text underlined with the u tag</u> to add a line underneath.</p>
    </main>

    <footer>
        <p>&copy; 2024 My Image and Video Editor</p>
    </footer>

    <script>
        let currentIndex = 0;
        const images = [
            "image1.jpg", "image2.jpg", "image3.jpg", "image4.jpg"
        ];
        const slideImage = document.getElementById("slideImage");
        const backgroundMusic = document.getElementById("backgroundMusic");
        const fileInput = document.getElementById("fileInput");
        const imageCanvas = document.getElementById("imageCanvas");
        const videoPlayer = document.getElementById("videoPlayer");
        let slideInterval;
        let currentFile = null;

        function startSlideshow() {
            if (!currentFile) {
                alert("Please upload an image first.");
                return;
            }

            if (slideInterval) {
                clearInterval(slideInterval);
            }

            slideInterval = setInterval(function () {
                currentIndex = (currentIndex + 1) % images.length;
                slideImage.src = images[currentIndex];
            }, 3000);
            backgroundMusic.play();
        }

        function stopSlideshow() {
            clearInterval(slideInterval);
            backgroundMusic.pause();
        }

        function toggleMusic() {
            if (backgroundMusic.paused) {
                backgroundMusic.play();
            } else {
                backgroundMusic.pause();
            }
        }

        document.getElementById("uploadForm").addEventListener("submit", function (e) {
            e.preventDefault();
            const file = fileInput.files[0];
            const message = document.getElementById("message");

            if (!file) {
                message.innerText = "Please select a file to upload.";
                return;
            }

            if (!file.type.startsWith("image/") && !file.type.startsWith("video/")) {
                message.innerText = "Only images and videos are allowed.";
                return;
            }

            if (file.type.startsWith("image/")) {
                currentFile = file;
                const reader = new FileReader();
                reader.onload = function (e) {
                    imageCanvas.style.display = "block";
                    const ctx = imageCanvas.getContext("2d");
                    const img = new Image();
                    img.onload = function () {
                        imageCanvas.width = img.width;
                        imageCanvas.height = img.height;
                        ctx.drawImage(img, 0, 0);
                    };
                    img.src = e.target.result;
                };
                reader.readAsDataURL(file);
            } else if (file.type.startsWith("video/")) {
                currentFile = file;
                const reader = new FileReader();
                reader.onload = function (e) {
                    videoPlayer.style.display = "block";
                    videoPlayer.src = e.target.result;
                };
                reader.readAsDataURL(file);
            }
        });

        function applyFilter() {
            if (!currentFile || !currentFile.type.startsWith("image/")) {
                alert("Please upload an image first.");
                return;
            }

            const ctx = imageCanvas.getContext("2d");
            ctx.filter = "grayscale(100%)"; // Change the filter if needed
            ctx.drawImage(imageCanvas, 0, 0);
        }

        function applyVideoEffect() {
            if (!currentFile || !currentFile.type.startsWith("video/")) {
                alert("Please upload a video first.");
                return;
            }

            videoPlayer.style.filter = "sepia(100%)"; // Adjust video effect as needed
        }

        function downloadFile() {
            if (!currentFile) {
                alert("Please upload a file first.");
                return;
            }

            const a = document.createElement('a');
            const url = URL.createObjectURL(currentFile);
            a.href = url;
            a.download = currentFile.name || "download";
            document.body.appendChild(a);
            a.click();
            document.body.removeChild(a);
        }
    </script>
</body>
</html>

