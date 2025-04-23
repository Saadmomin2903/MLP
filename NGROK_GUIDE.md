# Ngrok Setup and Usage Guide

## 1. Installation

### For macOS:

```bash
# Using Homebrew
brew install ngrok
```

### For Windows:

1. Download from https://ngrok.com/download
2. Extract the zip file
3. Add ngrok to your PATH

### For Linux:

```bash
# Using snap
snap install ngrok
```

## 2. Authentication

1. Create account at https://dashboard.ngrok.com/signup
2. Get your authtoken from https://dashboard.ngrok.com/get-started/your-authtoken
3. Configure ngrok with your token:

```bash
ngrok config add-authtoken YOUR_AUTH_TOKEN
```

## 3. Running the APIs with Ngrok

### Method 1: Using run_apis.py (Recommended)

```bash
# This will start all APIs and expose combined API through ngrok
python run_apis.py
```

### Method 2: Manual Setup

```bash
# 1. Start the APIs in separate terminals
python main.py          # Port 8000
python scraper.py       # Port 80010
python combined_api.py  # Port 8002

# 2. Start ngrok for combined API
ngrok http 8002
```

## 4. Testing the API

### Using curl

```bash
# Test combined API
curl -X POST "https://your-ngrok-url/analyze-facebook-comments" \
-H "Content-Type: application/json" \
-d '{"post_url": "https://www.facebook.com/your-post-url"}'
```

### Using Thunder Client

1. Create new request
2. Set Method: POST
3. URL: https://your-ngrok-url/analyze-facebook-comments
4. Headers:
   ```
   Content-Type: application/json
   ```
5. Body (JSON):
   ```json
   {
     "post_url": "https://www.facebook.com/your-post-url"
   }
   ```

## 5. Available Endpoints

### 1. Combined API (Port 8002)

- Endpoint: `/analyze-facebook-comments`
- Method: POST
- Purpose: Scrapes Facebook comments and analyzes sentiment

### 2. Sentiment Analysis API (Port 8000)

- Endpoint: `/predict`
- Method: POST
- Purpose: Analyzes sentiment of Marathi text

### 3. Facebook Scraper API (Port 8001)

- Endpoint: `/scrape-facebook-post`
- Method: POST
- Purpose: Only scrapes Facebook comments

## 6. Monitoring

1. Access ngrok dashboard:

```
http://localhost:4040
```

2. View in ngrok cloud:

- Go to https://dashboard.ngrok.com/cloud-edge/endpoints

## 7. Common Issues and Solutions

### 1. Multiple Tunnel Error

Error: "account limited to 1 simultaneous ngrok agent session"
Solution: Use only one tunnel or upgrade to paid plan

### 2. Authentication Failed

Solution: Re-run authentication command:

```bash
ngrok config add-authtoken YOUR_AUTH_TOKEN
```

### 3. Port Already in Use

Solution: Kill process using the port:

```bash
# For macOS/Linux
lsof -i :8002
kill -9 PID

# For Windows
netstat -ano | findstr :8002
taskkill /PID YOUR_PID /F
```

## 8. Best Practices

1. Always check ngrok status at http://localhost:4040
2. Keep your authtoken secure
3. Use HTTPS URLs for better security
4. Monitor your usage at dashboard.ngrok.com

## 9. Rate Limits (Free Plan)

- 1 simultaneous ngrok agent session
- 40 connections / minute
- 2 hours tunnel timeout

## 10. Example Usage

### Basic Example:

```bash
# Start combined API
python combined_api.py

# In new terminal, start ngrok
ngrok http 8002
```

### Testing Different Posts:

```bash
# Test with Facebook post
curl -X POST "https://your-ngrok-url/analyze-facebook-comments" \
-H "Content-Type: application/json" \
-d '{"post_url": "https://www.facebook.com/share/p/16QyiEfvMW/"}'
```

## 11. Shutdown

1. Press `Ctrl+C` in ngrok terminal
2. Press `Ctrl+C` in API terminals
3. Or kill all Python processes:

```bash
# macOS/Linux
pkill -f python

# Windows
taskkill /F /IM python.exe
```
