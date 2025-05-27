<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Smart YouTube Analyzer</title>
    <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;500;600&display=swap" rel="stylesheet">
    <link href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css" rel="stylesheet">
    <style>
        :root {
            --primary-color: #ff0000;
            --secondary-color: #065fd4;
            --bg-color: #f9f9f9;
            --card-bg: #ffffff;
            --text-primary: #030303;
            --text-secondary: #606060;
            --shadow: 0 2px 10px rgba(0,0,0,0.1);
        }

         {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Poppins', sans-serif;
            background-color: var(--bg-color);
            color: var(--text-primary);
            line-height: 1.6;
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 20px;
        }

        .header {
            text-align: center;
            margin-bottom: 30px;
            padding: 20px;
            background: var(--card-bg);
            border-radius: 15px;
            box-shadow: var(--shadow);
        }

        .header h1 {
            color: var(--primary-color);
            font-size: 2.5em;
            margin-bottom: 10px;
        }

        .input-section {
            display: flex;
            gap: 10px;
            margin: 20px auto;
            max-width: 800px;
        }

        .url-input {
            flex: 1;
            padding: 15px;
            border: 2px solid #e0e0e0;
            border-radius: 10px;
            font-size: 16px;
            transition: all 0.3s ease;
        }

        .url-input:focus {
            border-color: var(--primary-color);
            outline: none;
            box-shadow: 0 0 0 3px rgba(255,0,0,0.1);
        }

        .analyze-btn {
            padding: 15px 30px;
            background: var(--primary-color);
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 16px;
            cursor: pointer;
            transition: all 0.3s ease;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .analyze-btn:hover {
            background: #cc0000;
            transform: translateY(-2px);
        }

        .loading {
            display: none;
            text-align: center;
            padding: 20px;
        }

        .loading-spinner {
            width: 50px;
            height: 50px;
            border: 5px solid #f3f3f3;
            border-top: 5px solid var(--primary-color);
            border-radius: 50%;
            animation: spin 1s linear infinite;
            margin: 0 auto 15px;
        }

        .video-container {
            background: var(--card-bg);
            border-radius: 15px;
            padding: 20px;
            margin-bottom: 30px;
            box-shadow: var(--shadow);
        }

        .video-player {
            position: relative;
            padding-bottom: 56.25%;
            height: 0;
            overflow: hidden;
            border-radius: 10px;
        }

        .video-player iframe {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
        }

        .channel-info {
            display: flex;
            align-items: center;
            gap: 20px;
            margin: 20px 0;
            padding: 20px;
            background: #f8f8f8;
            border-radius: 10px;
        }

        .channel-avatar {
            width: 80px;
            height: 80px;
            border-radius: 50%;
            object-fit: cover;
        }

        .channel-details h2 {
            font-size: 1.5em;
            margin-bottom: 5px;
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .stat-card {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 10px;
            box-shadow: var(--shadow);
            text-align: center;
            transition: transform 0.3s ease;
        }

        .stat-card:hover {
            transform: translateY(-5px);
        }

        .stat-icon {
            font-size: 24px;
            color: var(--primary-color);
            margin-bottom: 10px;
        }

        .stat-value {
            font-size: 1.5em;
            font-weight: 600;
            color: var(--text-primary);
        }

        .stat-label {
            color: var(--text-secondary);
            font-size: 0.9em;
        }

        .content-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 20px;
            margin-bottom: 30px;
        }

        .content-card {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 15px;
            box-shadow: var(--shadow);
        }

        .content-card h3 {
            color: var(--secondary-color);
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .keywords-container {
            display: flex;
            flex-wrap: wrap;
            gap: 10px;
        }

        .keyword {
            background: #f0f0f0;
            padding: 8px 15px;
            border-radius: 20px;
            font-size: 0.9em;
            transition: all 0.3s ease;
            cursor: pointer;
        }

        .keyword:hover {
            background: var(--primary-color);
            color: white;
        }

        .flashcard {
            background: #fff;
            padding: 20px;
            border-radius: 10px;
            margin-bottom: 15px;
            cursor: pointer;
            transition: all 0.3s ease;
            border: 1px solid #e0e0e0;
        }

        .flashcard:hover {
            box-shadow: 0 5px 15px rgba(0,0,0,0.1);
        }

        .flashcard .question {
            font-weight: 500;
            margin-bottom: 10px;
        }

        .flashcard .answer {
            display: none;
            padding-top: 10px;
            border-top: 1px solid #e0e0e0;
            color: var(--text-secondary);
        }

        .flashcard.active .answer {
            display: block;
            animation: fadeIn 0.5s ease;
        }

        .hidden {
            display: none;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(-10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }

        .summary-section {
            background: var(--card-bg);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 30px;
            box-shadow: var(--shadow);
        }

        .summary-section h2 {
            color: var(--secondary-color);
            margin-bottom: 15px;
        }

        .summary-content {
            line-height: 1.8;
            color: var(--text-primary);
        }

        .key-points {
            margin-top: 20px;
            padding-top: 20px;
            border-top: 1px construire solid #e0e0e0;
        }

        .key-points h3 {
            color: var(--secondary-color);
            margin-bottom: 10px;
        }

        .key-point-item {
            display: flex;
            align-items: flex-start;
            gap: 10px;
            margin-bottom: 10px;
        }

        .key-point-item i {
            color: var(--primary-color);
            margin-top: 5px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>Smart YouTube Analyzer</h1>
            <p>Get detailed insights and analysis for any YouTube video</p>
        </div>

        <div class="input-section">
            <input type="text" class="url-input" id="urlInput" placeholder="Paste YouTube URL here...">
            <button class="analyze-btn" onclick="analyzeVideo()">
                <i class="fas fa-search"></i>
                Analyze
            </button>
        </div>

        <div id="loading" class="loading">
            <div class="loading-spinner"></div>
            <p>Analyzing video...</p>
        </div>

        <div id="results" class="hidden">
            <div class="video-container">
                <div class="video-player" id="player"></div>
            </div>

            <div class="channel-info">
                <img id="channelAvatar" class="channel-avatar" src="" alt="Channel Avatar">
                <div class="channel-details">
                    <h2 id="channelName"></h2>
                    <div id="subscriberCount"></div>
                </div>
            </div>

            <div class="stats-grid">
                <div class="stat-card">
                    <i class="fas fa-eye stat-icon"></i>
                    <div class="stat-value" id="viewCount"></div>
                    <div class="stat-label">Views</div>
                </div>
                <div class="stat-card">
                    <i class="fas fa-thumbs-up stat-icon"></i>
                    <div class="stat-value" id="likeCount"></div>
                    <div class="stat-label">Likes</div>
                </div>
                <div class="stat-card">
                    <i class="fas fa-comments stat-icon"></i>
                    <div class="stat-value" id="commentCount"></div>
                    <div class="stat-label">Comments</div>
                </div>
                <div class="stat-card">
                    <i class="fas fa-clock stat-icon"></i>
                    <div class="stat-value" id="duration"></div>
                    <div class="stat-label">Duration</div>
                </div>
            </div>

            <div class="summary-section">
                <h2><i class="fas fa-file-alt"></i> Video Summary</h2>
                <div class="summary-content" id="summary"></div>
                <div class="key-points">
                    <h3>Key Points</h3>
                    <div id="keyPoints"></div>
                </div>
            </div>

            <div class="content-grid">
                <div class="content-card">
                    <h3><i class="fas fa-chart-line"></i> Sentiment Analysis</h3>
                    <div id="sentiment"></div>
                </div>

                <div class="content-card">
                    <h3><i class="fas fa-tags"></i> Keywords</h3>
                    <div class="keywords-container" id="keywords"></div>
                </div>

                <div class="content-card">
                    <h3><i class="fas fa-question-circle"></i> Q&A</h3>
                    <div id="qa"></div>
                </div>

                <div class="content-card">
                    <h3><i class="fas fa-cards"></i> Flashcards</h3>
                    <div id="flashcards"></div>
                </div>
            </div>
        </div>
    </div>

    <script>
        const API_KEY = 'AIzaSyBZKy40tJA5nISlJo88fC-BjTtc3OdA71Y';

        function getVideoId(url) {
            const regExp = /^.*(youtu.be\/|v\/|u\/\w\/|embed\/|watch\?v=|&v=)([^#&?]*).*/;
            const match = url.match(regExp);
            return (match && match[2].length === 11) ? match[2] : null;
        }

        function formatNumber(num) {
            return num ? parseInt(num).toLocaleString() : 'N/A';
        }

        function formatDuration(duration) {
            if (!duration) return 'N/A';
            const match = duration.match(/PT(\d+H)?(\d+M)?(\d+S)?/);
            if (!match) return duration;
            
            const hours = (match[1] ? parseInt(match[1]) : 0);
            const minutes = (match[2] ? parseInt(match[2]) : 0);
            const seconds = (match[3] ? parseInt(match[3]) : 0);
            
            let result = '';
            if (hours > 0) result += hours + 'h ';
            if (minutes > 0 || hours > 0) result += minutes + 'm ';
            result += seconds + 's';
            
            return result;
        }

        function embedVideo(videoId) {
            const playerDiv = document.getElementById('player');
            playerDiv.innerHTML = `<iframe src="https://www.youtube.com/embed/${videoId}" frameborder="0" allowfullscreen></iframe>`;
        }

        function extractKeywords(text) {
            if (!text) return [];
            const words = text.toLowerCase().split(/\s+/);
            const commonWords = new Set(['the', 'and', 'to', 'of', 'a', 'in', 'is', 'it', 'you', 'that', 'he', 'was', 'for', 'on', 'are', 'with', 'as', 'I', 'his', 'they', 'be', 'at', 'one', 'have', 'this', 'from', 'or', 'had', 'by', 'not', 'word', 'but', 'what', 'some', 'we', 'can', 'out', 'other', 'were', 'all', 'there', 'when', 'up', 'use', 'your', 'how', 'said', 'an', 'each', 'she']);
            
            const wordCount = {};
            words.forEach(word => {
                if (word.length > 3 && !commonWords.has(word)) {
                    wordCount[word] = (wordCount[word] || 0) + 1;
                }
            });
            
            return Object.entries(wordCount)
                .sort((a, b) => b[1] - a[1])
                .slice(0, 10)
                .map(entry => entry[0]);
        }

        function generateQA(video, transcript) {
            const description = video.snippet.description || '';
            const title = video.snippet.title || '';
            const keywords = extractKeywords(description + ' ' + transcript);
            
            const qaList = [
                {
                    question: `What is the main topic of "${title}"?`,
                    answer: `The main topic of the video is ${keywords.slice(0, 3).join(', ')}, as derived from its description and content.`
                },
                {
                    question: "What is the primary focus or theme of the video?",
                    answer: description.length > 100 
                        ? `${description.substring(0, 100)}...`
                        : description || 'The video focuses on general content related to its title and channel.'
                },
                {
                    question: "What key information is shared in the video?",
                    answer: `The video shares insights related to ${keywords.slice(0, 5).join(', ')}, based on its transcript and description.`
                }
            ];
            
            return qaList.map((qa, i) => `
                <div class="flashcard" onclick="toggleFlashcard(${i})">
                    <div class="question">${qa.question}</div>
                    <div class="answer">${qa.answer}</div>
                </div>
            `).join('');
        }

        function generateFlashcards(video) {
            const cards = [
                { question: "What is the video about?", answer: video.snippet.description.substring(0, 100) + "..." },
                { question: "When was the video published?", answer: new Date(video.snippet.publishedAt).toLocaleDateString() },
                { question: "What is the channel name?", answer: video.snippet.channelTitle }
            ];
            
            return cards.map((card, i) => `
                <div class="flashcard" onclick="toggleFlashcard(${i + 3})">
                    <div class="question">${card.question}</div>
                    <div class="answer">${card.answer}</div>
                </div>
            `).join('');
        }

        async function fetchTranscript(videoId) {
            return "This would be the transcript of the video. In a real implementation, you would fetch this from a transcript API or use YouTube's auto-generated captions if available.";
        }

        async function analyzeVideo() {
            const url = document.getElementById('urlInput').value;
            const videoId = getVideoId(url);

            if (!videoId) {
                showNotification('Please enter a valid YouTube URL', 'error');
                return;
            }

            showLoading();

            try {
                const [videoData, transcriptData] = await Promise.all([
                    fetchVideoData(videoId),
                    fetchTranscript(videoId)
                ]);

                hideLoading();
                displayResults(videoData, transcriptData);
            } catch (error) {
                hideLoading();
                showNotification('Error analyzing video. Please try again.', 'error');
                console.error('Error:', error);
            }
        }

        function showLoading() {
            document.getElementById('loading').style.display = 'block';
            document.getElementById('results').classList.add('hidden');
        }

        function hideLoading() {
            document.getElementById('loading').style.display = 'none';
            document.getElementById('results').classList.remove('hidden');
        }

        async function fetchVideoData(videoId) {
            const response = await fetch(`https://www.googleapis.com/youtube/v3/videos?part=snippet,statistics,contentDetails&id=${videoId}&key=${API_KEY}`);
            const data = await response.json();
            
            if (!data.items || data.items.length === 0) {
                throw new Error('Video not found');
            }

            const channelId = data.items[0].snippet.channelId;
            const channelResponse = await fetch(`https://www.googleapis.com/youtube/v3/channels?part=snippet,statistics&id=${channelId}&key=${API_KEY}`);
            const channelData = await channelResponse.json();

            return {
                video: data.items[0],
                channel: channelData.items[0]
            };
        }

        function generateSummary(video, transcript) {
            const description = video.snippet.description;
            const title = video.snippet.title;
            
            const topics = extractKeywords(description)
                .slice(0, 3)
                .map(topic => `<div class="key-point-item">
                    <i class="fas fa-check-circle"></i>
                    <span>${topic.charAt(0).toUpperCase() + topic.slice(1)}</span>
                </div>`);

            return {
                summary: `${title} is a video that ${description.substring(0, 200)}...`,
                keyPoints: topics.join('')
            };
        }

        function displayResults(data, transcript) {
            const { video, channel } = data;

            embedVideo(video.id);

            document.getElementById('channelName').textContent = channel.snippet.title;
            document.getElementById('channelAvatar').src = channel.snippet.thumbnails.default.url;
            document.getElementById('subscriberCount').innerHTML = `
                <i class="fas fa-users"></i> 
                ${formatNumber(channel.statistics.subscriberCount)} subscribers
            `;

            document.getElementById('viewCount').textContent = formatNumber(video.statistics.viewCount);
            document.getElementById('likeCount').textContent = formatNumber(video.statistics.likeCount);
            document.getElementById('commentCount').textContent = formatNumber(video.statistics.commentCount);
            document.getElementById('duration').textContent = formatDuration(video.contentDetails.duration);

            const { summary, keyPoints } = generateSummary(video, transcript);
            document.getElementById('summary').textContent = summary;
            document.getElementById('keyPoints').innerHTML = keyPoints;

            const sentiment = analyzeSentiment(video.snippet.description);
            document.getElementById('sentiment').innerHTML = `
                <div class="sentiment-score">
                    <i class="fas fa-${sentiment.icon}"></i>
                    Overall sentiment: ${sentiment.label}
                </div>
                <div class="sentiment-details">${sentiment.details}</div>
            `;

            const keywords = extractKeywords(video.snippet.description);
            document.getElementById('keywords').innerHTML = keywords
                .map(keyword => `<span class="keyword">${keyword}</span>`)
                .join('');

            document.getElementById('qa').innerHTML = generateQA(video, transcript);

            document.getElementById('flashcards').innerHTML = generateFlashcards(video);
        }

        function analyzeSentiment(text) {
            if (!text) {
                return {
                    label: 'Neutral',
                    icon: 'meh',
                    details: 'Not enough text to analyze sentiment'
                };
            }
            
            const positiveWords = ['good', 'great', 'awesome', 'excellent', 'amazing', 'love', 'best', 'fantastic'];
            const negativeWords = ['bad', 'poor', 'terrible', 'worst', 'hate', 'awful', 'disappointing'];

            let positiveCount = 0;
            let negativeCount = 0;

            text.toLowerCase().split(' ').forEach(word => {
                if (positiveWords.includes(word)) positiveCount++;
                if (negativeWords.includes(word)) negativeCount++;
            });

            if (positiveCount > negativeCount) {
                return {
                    label: 'Positive',
                    icon: 'smile',
                    details: `Found ${positiveCount} positive expressions and ${negativeCount} negative expressions`
                };
            } else if (negativeCount > positiveCount) {
                return {
                    label: 'Negative',
                    icon: 'frown',
                    details: `Found ${negativeCount} negative expressions and ${positiveCount} positive expressions`
                };
            }
            return {
                label: 'Neutral',
                icon: 'meh',

                details: 'The content appears to be mostly neutral in tone'
            };
        }

        function showNotification(message, type) {
            alert(message);
        }

        function toggleFlashcard(index) {
            const flashcards = document.querySelectorAll('.flashcard');
            if (index < flashcards.length) {
                flashcards[index].classList.toggle('active');
            }
        }

        document.getElementById('urlInput').addEventListener('keypress', function(e) {
            if (e.key === 'Enter') {
                analyzeVideo();
            }
        });
    </script>
</body>
</html># youtube-video-analyzer
