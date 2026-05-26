# AI API Gateway

Unified API gateway for multiple AI models including OpenAI, Claude, Gemini, and Llama.

## Features

✅ **Multi-model support** - OpenAI, Claude, Gemini, Llama
✅ **Unified interface** - Single API format for all models
✅ **User management** - Registration, login, API key management
✅ **Billing system** - Automatic billing, balance management, recharge
✅ **Request logging** - Complete request logs and statistics
✅ **Rate limiting** - Prevent abuse
✅ **Load balancing** - Support multiple API keys
✅ **Admin dashboard** - User statistics and log queries

## Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/PonchillLee/ai-api-gateway.git
cd ai-api-gateway
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
```

### 3. Configure environment variables

```bash
cp .env.example .env
# Edit .env and add your API keys
```

### 4. Run the application

```bash
# Local development
uvicorn app.main:app --reload

# Or with Docker
docker-compose up -d
```

Access API docs at http://localhost:8000/docs

## API Documentation

### Authentication

#### Register
```bash
POST /api/v1/auth/register
{
  "username": "user",
  "email": "user@example.com",
  "password": "password123"
}
```

#### Login
```bash
POST /api/v1/auth/login
{
  "email": "user@example.com",
  "password": "password123"
}
```

### Chat

#### Send message
```bash
POST /api/v1/chat/completions
Authorization: Bearer <token>
{
  "provider": "openai",
  "model": "gpt-4",
  "messages": [
    {
      "role": "user",
      "content": "Hello"
    }
  ],
  "temperature": 0.7,
  "max_tokens": 1000
}
```

### Models

#### Get all models
```bash
GET /api/v1/models/
```

#### Get provider models
```bash
GET /api/v1/models/openai
```

### API Keys

#### Create API key
```bash
POST /api/v1/keys/
Authorization: Bearer <token>
{
  "name": "My API Key"
}
```

#### List API keys
```bash
GET /api/v1/keys/
Authorization: Bearer <token>
```

#### Delete API key
```bash
DELETE /api/v1/keys/{key_id}
Authorization: Bearer <token>
```

### Admin

#### Top up balance
```bash
POST /api/v1/admin/charge
Authorization: Bearer <token>
{
  "amount": 10000
}
```

#### Get statistics
```bash
GET /api/v1/admin/stats
Authorization: Bearer <token>
```

#### Get logs
```bash
GET /api/v1/admin/logs?limit=50
Authorization: Bearer <token>
```

## Pricing

Default pricing with 20% markup:

| Model | Input | Output |
|-------|-------|--------|
| GPT-4 | $0.03/1K | $0.06/1K |
| GPT-3.5-turbo | $0.0005/1K | $0.0015/1K |
| Claude-3-Opus | $0.015/1K | $0.075/1K |
| Gemini-Pro | $0.0005/1K | $0.0015/1K |

## Deployment

### Docker

```bash
docker build -t ai-gateway .
docker run -d -p 8000:8000 \
  -e OPENAI_API_KEY=sk-xxx \
  -e ANTHROPIC_API_KEY=sk-ant-xxx \
  ai-gateway
```

### Nginx Reverse Proxy

```nginx
server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://localhost:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

## Security Notes

1. Change the SECRET_KEY in .env
2. Use HTTPS in production
3. Set appropriate rate limits
4. Regularly backup the database
5. Monitor logs for suspicious activity

## License

MIT