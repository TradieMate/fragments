# Render Deployment Guide

This guide will help you deploy the Fragments application to Render.

## Prerequisites

1. A [Render](https://render.com) account
2. An [E2B API key](https://e2b.dev/) (required)
3. At least one LLM provider API key (OpenAI, Anthropic, etc.)

## Deployment Steps

### 1. Fork or Clone Repository

Fork this repository to your GitHub account or ensure you have access to deploy from your repository.

### 2. Create a New Web Service on Render

1. Go to your [Render Dashboard](https://dashboard.render.com/)
2. Click "New +" and select "Web Service"
3. Connect your GitHub repository
4. Configure the service:
   - **Name**: Choose a name for your app
   - **Environment**: Node
   - **Build Command**: `npm install && npm run build`
   - **Start Command**: `npm start`

### 3. Set Environment Variables

In the Render dashboard, go to your service's Environment tab and add the following variables:

#### Required Variables:
```
E2B_API_KEY=your-e2b-api-key
OPENAI_API_KEY=your-openai-api-key
ANTHROPIC_API_KEY=your-anthropic-api-key
```

#### Recommended Variables:
```
NEXT_PUBLIC_SITE_URL=https://your-app-name.onrender.com
RATE_LIMIT_MAX_REQUESTS=10
RATE_LIMIT_WINDOW=60000
```

#### Optional Variables (for enhanced functionality):
```

# Upstash KV for rate limiting and short URLs
KV_REST_API_URL=your-kv-url
KV_REST_API_TOKEN=your-kv-token

# Supabase for authentication
SUPABASE_URL=your-supabase-url
SUPABASE_ANON_KEY=your-supabase-anon-key

# PostHog for analytics
NEXT_PUBLIC_POSTHOG_KEY=your-posthog-key
NEXT_PUBLIC_POSTHOG_HOST=your-posthog-host

# Configuration flags
NEXT_PUBLIC_NO_API_KEY_INPUT=true  # Disable API key input
NEXT_PUBLIC_NO_BASE_URL_INPUT=true # Disable base URL input
NEXT_PUBLIC_HIDE_LOCAL_MODELS=true # Hide local models
```

### 4. Deploy

1. Click "Create Web Service"
2. Render will automatically build and deploy your application
3. Once deployed, your app will be available at `https://your-app-name.onrender.com`

## Important Notes

### Port Configuration
- The application is configured to bind to `0.0.0.0` and use the `PORT` environment variable
- Render automatically sets the `PORT` variable, so no manual configuration is needed

### E2B Templates
- The `sandbox-templates/` directory contains E2B sandbox configurations
- These are required for the application to function properly
- Do not remove this directory

### Rate Limiting
- Without Upstash KV, rate limiting will be basic
- For production use, consider setting up Upstash KV for better rate limiting

### Authentication
- Without Supabase, the app will work but won't have user authentication
- Users can still use the app by providing their own API keys

## Troubleshooting

### Build Issues
- Ensure all required environment variables are set
- Check the build logs in Render dashboard

### Runtime Issues
- Check the service logs in Render dashboard
- Verify E2B API key is valid and has sufficient credits
- Ensure at least one LLM provider API key is configured

### Performance
- Consider upgrading to a paid Render plan for better performance
- Monitor usage and adjust rate limits as needed

## Security Considerations

1. **API Keys**: Never commit API keys to your repository
2. **Rate Limiting**: Implement proper rate limiting to prevent abuse
3. **CORS**: The app is configured to allow CORS for iframe embedding
4. **Environment Variables**: Use Render's environment variable system for sensitive data

## Support

For issues related to:
- **E2B**: Check [E2B documentation](https://e2b.dev/docs)
- **Render**: Check [Render documentation](https://render.com/docs)
- **Application**: Check the application logs and ensure all required environment variables are set