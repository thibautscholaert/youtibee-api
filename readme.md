# Youtibee API

A Flask-based API service for downloading YouTube audio content with rate limiting and authentication support.

## Features

- YouTube audio download functionality using yt-dlp
- Google OAuth2 authentication
- Rate limiting with Redis
- Proxy support for downloads
- Docker containerization
- CORS support for web applications

## Prerequisites

- Python 3.9+
- FFmpeg
- Redis server
- Docker (optional)

## Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/youtibee-api.git
cd youtibee-api
```

2. Install dependencies:
```bash
pip install -r requirements.txt
```

3. Set up environment variables:
```bash
export REDIS_URL="redis://your-redis-host:6379"
export SECRET="your-secret-key"
export YT_COOKIE_BASE64="your-base64-encoded-cookies"
export RATE_LIMIT=5  # Optional, defaults to 5
export TIME_WINDOW=60  # Optional, defaults to 60 seconds
```

## Extracting YouTube Cookies

To access age-restricted or private content, you'll need to provide YouTube cookies. Here's how to extract them:

1. Open a new private/incognito window in your browser
2. Log into YouTube
3. Open a new tab and **close the YouTube tab**
4. Export `youtube.com` cookies from your browser
5. Convert the cookies to base64:
```bash
cat cookies.txt | base64 > cookies.b64.txt
```
6. Set the base64-encoded cookies as the `YT_COOKIE_BASE64` environment variable

⚠️ **Caution**: Using your account with yt-dlp runs the risk of it being banned. Be mindful of request rates and download amounts. Consider using a throwaway account for this purpose.

For more detailed information, see the [yt-dlp documentation on exporting YouTube cookies](https://github.com/yt-dlp/yt-dlp/wiki/Extractors#exporting-youtube-cookies).

## Docker Deployment

Build and run using Docker:

```bash
docker build -t youtibee-api .
docker run -p 5000:5000 \
  -e REDIS_URL="redis://your-redis-host:6379" \
  -e SECRET="your-secret-key" \
  -e YT_COOKIE_BASE64="your-base64-encoded-cookies" \
  youtibee-api
```

## API Endpoints

### GET /ping
Health check endpoint.

### GET /rate-limit
Check current rate limit status. Requires Bearer token authentication.

### GET /download/audio
Download audio from YouTube URL. Parameters:
- `url`: YouTube video URL
- `secret`: Base64 encoded secret key

Headers:
- `Authorization`: Bearer token for authentication

## Rate Limiting

The API implements rate limiting per user:
- Default limit: 5 requests per minute
- Configurable via environment variables
- Uses Redis for tracking request counts

## Security

- Google OAuth2 authentication
- Secret key validation for downloads
- CORS protection with allowed origins
- Rate limiting to prevent abuse

## Proxy Support

The API supports using proxies for downloads:
- Load proxies from `proxy.txt`
- Automatic proxy testing and selection
- SOCKS5 proxy support via environment variables

## License

No license set. All rights reserved.

## Contributing

N/A