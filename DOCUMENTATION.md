# Fake News Detection System: Documentation

## 1. Project Overview
This project is an end-to-end Machine Learning web application designed to identify fraudulent news articles. It allows users to input a news URL or paste raw text, which is then processed through a hybrid system combining a Logistic Regression model and a heuristic signal layer to predict its veracity (**REAL**, **FAKE**, or **UNCERTAIN**).

## 2. Problem Statement
The digital age has led to an explosion of information, unfortunately accompanied by "fake news"—misleading content masked as legitimate journalism. Automated detection is essential to maintain information integrity and prevent the spread of misinformation that can impact social stability and decision-making.

## 3. System Architecture
The application follows a standard Web-ML architecture:
- **Frontend**: A modern, responsive UI built with HTML5, Vanilla CSS3 (glassmorphism/dark mode), and JavaScript.
- **Backend**: A Flask (Python) server handling API requests, scraping, and model inference.
- **ML Pipeline**: A TF-IDF vectorization and Logistic Regression model trained on a curated dataset.
- **Scraper Engine**: Utilizing `BeautifulSoup` and `requests` with rotation headers to bypass simple bot detection.
- **Reliability Layer**: Logic-gate for short inputs and an "UNCERTAIN" classification for low-confidence results.

## 4. Technical Methodology

### A. Data Collection & Training
The system uses the `model_train.py` script to:
1. Load a supervised dataset (`fake_real_news.csv`).
2. Train a **Logistic Regression** classifier.
3. Feature Extraction: Uses **TF-IDF (Term Frequency-Inverse Document Frequency)** with `ngram_range=(1, 2)` to capture both single words and common phrases.

### B. Text Preprocessing
Implemented in `utils.py`, the preprocessing pipeline includes:
- Lowercasing and URL removal.
- Punctuation removal via Regex.
- **NLTK Integration**: Tokenization and removal of English stop-words.
- Minimum word length filtering to remove noise.

### C. Hybrid Heuristic Layer
A unique feature of this project is the `compute_heuristic_adjustment` function. It stabilizes predictions for small datasets by:
- **Trusted Domains**: Checking if the URL belongs to known reliable sources (e.g., BBC, Reuters, The Hindu).
- **Signal Terms**: Scanning for "Real Signals" VS "Fake/Sensational Signals" (e.g., *war, scandal, miracle*).
- **Uncertainty Handling**: Results with confidence between 45% and 55% are flagged as "UNCERTAIN" to avoid misleading binary predictions.
- **Length Validation**: Rejects manual inputs shorter than 20 words to ensure statistical significance.

## 5. Web Application Features
- **URL Mode**: Automatically extracts titles and paragraph content from web addresses.
- **Text Mode**: Allows manual input if a website blocks automated scraping.
- **Live Metadata**: Displays word counts, article previews, and confidence scores.
- **Responsive Design**: Optimized for both Desktop and Mobile viewports.

## 6. Performance Indicators
*Note: Current performance based on default small dataset.*
- **Accuracy**: Dynamic accuracy based on training outcome.
- **Classes**: Multi-state classification (REAL, FAKE, UNCERTAIN).
- **Inference Time**: Fast (< 1.5 seconds per article).

## 7. Installation & Deployment
1. **Dependencies**: `pip install -r requirements.txt`
2. **Setup**: Run `python model_train.py` to generate the `model.pkl`.
3. **Execution**: Start the server using `python app.py`.
4. **Vercel Implementation**: The project includes `vercel.json` and serverless-friendly `NLTK_DATA` handling for cloud deployment.

## 8. Conclusion & Future Scope
This project serves as a robust proof-of-concept for automated misinformation detection. Future enhancements include:
- Deep Learning integration (LSTMs or BERT Transformers).
- Multi-language support.
- Cross-referencing news facts with external Fact-Checking APIs.
