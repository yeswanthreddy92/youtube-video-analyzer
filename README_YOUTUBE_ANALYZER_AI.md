
# YouTube Video Analyzer AI Agent

The **YouTube Video Analyzer AI Agent** is a comprehensive tool designed to extract and analyze key insights from YouTube videos using advanced AI technologies. This application enables transcription, summarization, sentiment analysis, keyword extraction, and optional translation and email distribution of the results.

---

## Features

- **Audio Transcription**: Converts speech from YouTube videos into accurate text using Whisper ASR.
- **Summarization**: Generates concise summaries of the transcribed text using state-of-the-art language models.
- **Sentiment Analysis**: Identifies the overall emotional tone (positive, negative, or neutral) of the video content.
- **Keyword Extraction**: Highlights important terms and phrases to aid in content indexing and comprehension.
- **Translation (Optional)**: Supports translation of the summary into various languages.
- **Email Delivery (Optional)**: Sends the summarized insights to a specified email address.
- **Question Answering**: Enables users to ask specific questions based on the content of the video.
- **Web Interface**: Intuitive user interface built with React and Tailwind CSS for seamless interaction.

---

## Technology Stack

- **Frontend**: React.js, Tailwind CSS
- **Backend**: FastAPI
- **Machine Learning Models**:
  - Whisper (for transcription)
  - Transformers (for summarization, sentiment analysis, and question answering)
- **Deployment Options**: Replit, Render, Vercel, or other platforms

---

## Project Structure

```
youtube-video-analyzer-ai-agent/
├── backend/
│   ├── main.py
│   ├── routes/
│   └── utils/
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   └── pages/
│   └── public/
├── requirements.txt
├── package.json
├── .gitignore
└── README.md
```

---

## Setup Instructions

### Prerequisites

- Python 3.8+
- Node.js 14+
- pip and npm

### Backend Setup

```bash
cd backend
pip install -r requirements.txt
uvicorn main:app --reload
```

### Frontend Setup

```bash
cd frontend
npm install
npm run dev
```

> Ensure the frontend is configured to use the correct backend API URL (e.g., http://localhost:8000 or deployed URL).

---

## Usage

1. Enter a YouTube video URL.
2. Click "Analyze" to initiate processing.
3. View results including transcription, summary, sentiment, and keywords.
4. Optionally translate or email the summary.

---

## Deployment

- **Frontend**: Deploy on Vercel or Replit
- **Backend**: Deploy on Render, Replit, or any cloud platform supporting FastAPI

Ensure proper configuration of CORS and environment variables for deployment.

---

## Contribution

Contributions are welcome. To contribute:

1. Fork the repository
2. Create a feature branch
3. Commit your changes
4. Open a pull request

---

## License

This project is licensed under the MIT License.  
© 2025 Ch Yeswanth Reddy
        prem kumar
        daniel
        yaswanth
        mani

---

## Contact

**Developer**: Ch Yeswanth Reddy  
**GitHub**: [github.com/yeswanth92](https://github.com/yeswanth92)  
**Email**: chiralayeswanthreddy@gmail.com
