# File Transfer - Temporary File Sharing App

A complete, production-ready temporary file-sharing web application inspired by filetransfer.io. Share files securely using 6-digit codes with automatic deletion after 10 minutes.

## Features

- **Secure Code-Based Access**: No public links - files are only accessible with a unique 6-digit code
- **Automatic Deletion**: Files are permanently deleted after exactly 10 minutes
- **Drag & Drop Upload**: Easy file upload with progress tracking
- **Multiple File Support**: Upload up to 10 files (max 200MB total)
- **Download Options**: Download individual files or all files as a ZIP
- **Live Countdown Timer**: See exactly how much time remains before deletion
- **No User Accounts**: Completely anonymous and temporary
- **Responsive Design**: Works on desktop and mobile devices

## Architecture

This is a monorepo with two separate projects:

### Frontend (`/frontend`)
- **Technology**: Next.js 14 with Pages Router
- **Build**: Static export (`next export`) for GitHub Pages deployment
- **Features**: 
  - Drag-and-drop file upload
  - Real-time progress bars
  - Countdown timers
  - Clipboard integration
  - Responsive UI

### Backend (`/backend`)
- **Technology**: Node.js + Express
- **Deployment**: Render.com Web Service compatible
- **Features**:
  - File upload with size/count validation
  - Unique code generation (no collisions)
  - Automatic 10-minute cleanup
  - On-the-fly ZIP generation
  - In-memory transfer tracking

## Getting Started

### Prerequisites

- Node.js 16+ and npm

### Installation

1. Clone the repository:
```bash
git clone https://github.com/SystemInfomation/file.git
cd file
```

2. Install backend dependencies:
```bash
cd backend
npm install
```

3. Install frontend dependencies:
```bash
cd ../frontend
npm install
```

### Running Locally

1. Start the backend server:
```bash
cd backend
npm start
```
The backend will run on `http://localhost:3001`

2. In a new terminal, start the frontend:
```bash
cd frontend
npm run dev
```
The frontend will run on `http://localhost:3000`

3. Open `http://localhost:3000` in your browser

### Development Mode

For backend hot-reload:
```bash
cd backend
npm run dev
```

## Deployment

**Live Application**: 
- **Frontend**: https://forsyth-county.github.io/file/
- **Backend**: https://file-lc4x.onrender.com

### Backend (Render.com)

The backend is deployed at `https://file-lc4x.onrender.com`

Configuration:
- **Root Directory**: `backend`
- **Build Command**: `npm install`
- **Start Command**: `npm start`
- **Environment Variables**: 
  - `PORT`: (auto-set by Render)
  - `UPLOADS_DIR`: `/tmp`
  - `ALLOWED_ORIGINS`: `https://forsyth-county.github.io`

### Frontend (GitHub Pages)

The frontend is deployed at `https://forsyth-county.github.io/file/`

#### Automatic Deployment via GitHub Actions

This repository includes a GitHub Actions workflow that automatically deploys to GitHub Pages when you push to the main branch.

**Configuration**:
1. GitHub Pages is configured to use GitHub Actions as the source
2. The workflow builds the Next.js app with `basePath: '/file'` 
3. Backend URL is hardcoded as `https://file-lc4x.onrender.com`
4. Deploys the static output to GitHub Pages

For detailed deployment instructions, see [DEPLOYMENT.md](DEPLOYMENT.md).

## Configuration

### Backend Environment Variables

- `PORT`: Server port (default: 3001)
- `UPLOADS_DIR`: Directory for temporary file storage (default: `./uploads`)

### Frontend Environment Variables

- `NEXT_PUBLIC_BACKEND_URL`: Backend API URL
  - Production: `https://file-lc4x.onrender.com` (hardcoded in GitHub Actions)
  - Development: `http://localhost:3001` (default)

### Important Notes for GitHub Pages Deployment

1. **Base Path**: The app is configured with `basePath: '/file'` in `next.config.js` to work with GitHub Pages repository deployment at `https://forsyth-county.github.io/file/`
2. **CORS Configuration**: Backend allows requests from `https://forsyth-county.github.io` (and `http://localhost:3000` for local development)
3. **HTTPS Required**: Both frontend and backend use HTTPS in production
4. **Static Export**: Next.js is configured with `output: 'export'` for static site generation
5. **Asset URLs**: All static assets are prefixed with `/file/` base path

## Usage

### Sending Files

1. Go to the home page
2. Drag & drop files or click to select (max 10 files, 200MB total)
3. Click "Upload Files"
4. Get your 6-digit code and share it with the recipient
5. Files will be automatically deleted after 10 minutes

### Receiving Files

1. Click "Receive Files" or go to `/receive`
2. Enter the 6-digit code
3. Download individual files or all as ZIP
4. Files remain available until the 10-minute timer expires

## Technical Details

### File Storage
- Files are stored with UUID filenames to prevent collisions
- Original filenames are preserved in metadata
- Automatic cleanup after 10 minutes, even if not downloaded

### Code Generation
- Random 6-digit codes (100000-999999)
- Collision detection and regeneration
- No leading zeros for better UX

### Security
- No permanent storage
- No public file listing
- CORS enabled (configure for production)
- File size and count validation

## Project Structure

```
file/
├── backend/
│   ├── server.js           # Express server
│   ├── package.json
│   └── .gitignore
├── frontend/
│   ├── pages/
│   │   ├── index.js        # Upload page
│   │   ├── success.js      # Success with code
│   │   ├── receive.js      # Enter code
│   │   └── download.js     # Download files
│   ├── styles/
│   │   └── globals.css     # Global styles
│   ├── config.js           # Backend URL config
│   ├── next.config.js      # Next.js config
│   └── package.json
└── README.md
```

## License

MIT
