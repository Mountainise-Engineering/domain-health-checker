# domain-health-checker
App to check the Spam Filters &amp; Domain Health. Run an instant email deliverability test on your domain or IP. Get clear insights, spam filter results, and inbox placement data with AI-powered recommendations.

## Features

- **Inbox Placement Testing** - Check if your emails land in inbox, spam, or promotions folders
- **Spam Filter Analysis** - Identify spam triggers and get detailed reports
- **Domain Health Check** - Analyze SPF, DKIM, DMARC records and domain reputation
- **AI-Powered Recommendations** - Get specific steps to improve deliverability
- **Real-time Analysis** - Instant results with comprehensive testing
- **Multi-Provider Testing** - Test across Gmail, Outlook, Yahoo, and other major email providers

## Architecture

This application follows the same architecture pattern as other MountAInise apps:

- **Frontend**: React + Vite + TailwindCSS + React Router
- **Backend**: Express.js + Node.js
- **Integration**: N8N workflow for email deliverability testing

## Project Structure

```
email-deliverability-app/
├── backend/
│   ├── index.js           # Express server with API endpoints
│   ├── package.json       # Backend dependencies
│   └── .env.example       # Environment variables template
├── frontend/
│   ├── src/
│   │   ├── components/
│   │   │   └── HomePage.jsx      # Landing page
│   │   ├── pages/
│   │   │   ├── EmailTestPage.jsx # Test form page
│   │   │   └── ResultsPage.jsx   # Results display
│   │   ├── App.jsx        # Main app with routing
│   │   ├── main.jsx       # React entry point
│   │   └── App.css        # Global styles
│   ├── index.html         # HTML entry point
│   ├── vite.config.js     # Vite configuration
│   └── package.json       # Frontend dependencies
└── README.md              # This file
```

## Setup Instructions

### Prerequisites

- Node.js (v18 or higher)
- npm or yarn
- N8N instance running (for email deliverability testing)

### Backend Setup

1. Navigate to the backend directory:
```bash
cd backend
```

2. Install dependencies:
```bash
npm install
```

3. Create a `.env` file from `.env.example`:
```bash
cp .env.example .env
```

4. Configure your `.env` file:
```env
PORT=3001
SESSION_SECRET=your-secret-key
N8N_WEBHOOK_URL=http://localhost:5678/webhook/email-deliverability-test
```

5. Start the backend server:
```bash
npm start
```

The backend will run on `http://localhost:3001`

### Frontend Setup

1. Navigate to the frontend directory:
```bash
cd frontend
```

2. Install dependencies:
```bash
npm install
```

3. Start the development server:
```bash
npm run dev
```

The frontend will run on `http://localhost:5174`

## Usage

1. **Open the application** - Navigate to `http://localhost:5174` in your browser

2. **Choose test type** - Select whether you want to test a domain, email address, or IP address

3. **Enter test data** - Input your domain name (e.g., example.com), email address, or IP address

4. **Run the test** - Click "Run Deliverability Test" to start the analysis

5. **View results** - The app will send the test data to your N8N workflow and display:
   - Overall deliverability score
   - SPF, DKIM, DMARC record validation
   - Blacklist status
   - Inbox placement percentages
   - AI-powered recommendations for improvement

## N8N Workflow Setup

To use this application, you need to create an N8N workflow that:

1. **Receives the webhook** - Set up a Webhook node to receive test data
2. **Performs email deliverability tests**:
   - Check DNS records (SPF, DKIM, DMARC)
   - Test domain/IP reputation
   - Check blacklist status
   - Simulate inbox placement
3. **Analyzes results** - Use AI to generate recommendations
4. **Returns response** - Send results back to the application

### Example N8N Response Format

```json
{
  "overallScore": 85,
  "spf": {
    "valid": true,
    "record": "v=spf1 include:_spf.google.com ~all"
  },
  "dkim": {
    "valid": true
  },
  "dmarc": {
    "valid": true,
    "policy": "quarantine"
  },
  "blacklist": {
    "listed": false,
    "count": 0
  },
  "inboxPlacement": {
    "inbox": 90,
    "spam": 5,
    "promotions": 5
  },
  "recommendations": "Your email authentication is properly configured. Consider implementing DMARC policy 'reject' for stronger security."
}
```

## API Endpoints

### Backend API

- `GET /health` - Health check endpoint
- `POST /test-email` - Run email deliverability test
  - Body: `{ domain, email, ipAddress, testType }`
- `GET /last-test` - Get last test results from session
- `POST /clear-session` - Clear session data

## Technology Stack

### Frontend
- React 19
- React Router v7
- TailwindCSS v4
- Vite v7
- React Markdown (for displaying recommendations)

### Backend
- Express v5
- Axios (for N8N webhook calls)
- express-session (session management)
- dotenv (environment variables)
- CORS enabled

## Development

### Running in Development Mode

**Terminal 1 - Backend:**
```bash
cd backend
npm start
```

**Terminal 2 - Frontend:**
```bash
cd frontend
npm run dev
```

### Building for Production

**Frontend:**
```bash
cd frontend
npm run build
```

The production build will be in `frontend/dist/`

## Configuration

### Backend Configuration

Edit `backend/.env`:
- `PORT` - Backend server port (default: 3001)
- `SESSION_SECRET` - Secret key for session management
- `N8N_WEBHOOK_URL` - Your N8N webhook URL for email deliverability testing

### Frontend Configuration

Edit `frontend/vite.config.js`:
- `server.port` - Frontend development server port (default: 5174)
- `server.open` - Auto-open browser on start

## Troubleshooting

### Backend Issues

**Connection refused to N8N:**
- Ensure N8N is running
- Check the `N8N_WEBHOOK_URL` in your `.env` file
- Verify the webhook URL is correct

**Session not working:**
- Check that `SESSION_SECRET` is set in `.env`
- Clear browser cookies and try again

### Frontend Issues

**Cannot connect to backend:**
- Ensure backend is running on `http://localhost:3001`
- Check CORS settings in `backend/index.js`

**Styles not loading:**
- Run `npm install` again
- Clear browser cache
- Check TailwindCSS configuration

## Contributing

This application follows the MountAInise app architecture pattern. When making changes:

1. Maintain the existing structure
2. Follow React best practices
3. Use TailwindCSS for styling
4. Keep the N8N integration pattern
5. Update documentation as needed

## License

ISC

## Support

For issues and questions, please contact the MountAInise team.
