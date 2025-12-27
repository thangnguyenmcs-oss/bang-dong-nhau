# Bang Dong Nhau 🎮

A fun and interactive web-based game built with modern web technologies, featuring real-time multiplayer capabilities powered by Firebase.

## Table of Contents

- [Game Features](#game-features)
- [Tech Stack](#tech-stack)
- [Setup Instructions](#setup-instructions)
- [Firebase Configuration Guide](#firebase-configuration-guide)
- [Deployment to GitHub Pages](#deployment-to-github-pages)
- [Playing the Game](#playing-the-game)
- [Contributing](#contributing)
- [License](#license)

## Game Features

### Core Features
- 🎯 **Interactive Gameplay** - Engaging game mechanics designed for entertainment and fun
- 👥 **Multiplayer Support** - Real-time multiplayer capabilities with Firebase integration
- 🔥 **Firebase Integration** - Seamless cloud backend with Realtime Database and Authentication
- 📱 **Responsive Design** - Works perfectly on desktop, tablet, and mobile devices
- ⚡ **Real-time Updates** - Instant synchronization of game state across all players
- 💾 **Data Persistence** - Game progress and scores stored in Firebase

### Technical Features
- Modern web technologies (HTML5, CSS3, JavaScript)
- Firebase Realtime Database for live data synchronization
- Firebase Authentication for user management
- GitHub Pages deployment ready
- Responsive and mobile-friendly UI
- Cross-browser compatibility

## Tech Stack

- **Frontend**: HTML5, CSS3, JavaScript (ES6+)
- **Backend**: Firebase (Realtime Database, Authentication, Hosting)
- **Deployment**: GitHub Pages
- **Version Control**: Git & GitHub

## Setup Instructions

### Prerequisites

Before you begin, ensure you have the following installed:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/) (version 14 or higher)
- [npm](https://www.npmjs.com/) (comes with Node.js)
- A [Firebase Account](https://console.firebase.google.com/)
- A [GitHub Account](https://github.com/)

### Step 1: Clone the Repository

```bash
git clone https://github.com/thangnguyenmcs-oss/bang-dong-nhau.git
cd bang-dong-nhau
```

### Step 2: Install Dependencies

```bash
npm install
```

### Step 3: Create Environment Configuration

Create a `.env` file in the root directory:

```bash
cp .env.example .env
```

Then edit the `.env` file with your Firebase configuration details (see Firebase Configuration Guide below).

### Step 4: Run Locally

For development with local server:

```bash
npm start
```

The application will be available at `http://localhost:3000` (or another port if specified in your configuration).

### Step 5: Build for Production

```bash
npm run build
```

This creates an optimized production build in the `dist/` or `build/` directory.

## Firebase Configuration Guide

### Step 1: Create a Firebase Project

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Create a new project"** or **"Add project"**
3. Enter your project name (e.g., "bang-dong-nhau")
4. Follow the setup wizard
5. Enable Google Analytics if desired (optional)
6. Click **"Create project"**

### Step 2: Set Up Firebase Realtime Database

1. In the Firebase Console, select your project
2. Navigate to **"Realtime Database"** from the left menu
3. Click **"Create Database"**
4. Choose your database location (select one closest to your users)
5. Start in **"Test Mode"** for development (for production, set up proper security rules)
6. Click **"Enable"**

### Step 3: Set Up Firebase Authentication

1. Go to **"Authentication"** in the left menu
2. Click **"Get Started"**
3. Enable authentication methods:
   - **Email/Password**: Click the Email/Password provider and enable it
   - **Google**: (Optional) Enable for seamless Google login
   - **Anonymous**: (Optional) For anonymous gameplay
4. Save changes

### Step 4: Get Your Firebase Configuration

1. Go to **Project Settings** (gear icon at the top left)
2. Under **"Your apps"** section, click **"Web"** icon (</> symbol)
3. Register your web app
4. Copy the Firebase configuration object

### Step 5: Configure Your Application

Add the Firebase configuration to your `.env` file:

```env
VITE_FIREBASE_API_KEY=your_api_key_here
VITE_FIREBASE_AUTH_DOMAIN=your_project_id.firebaseapp.com
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_project_id.appspot.com
VITE_FIREBASE_MESSAGING_SENDER_ID=your_messaging_sender_id
VITE_FIREBASE_APP_ID=your_app_id
VITE_FIREBASE_DATABASE_URL=https://your_project_id.firebaseio.com
```

### Step 6: Set Up Firebase Security Rules

For development (Test Mode):

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

For production, implement proper security rules:

```json
{
  "rules": {
    "players": {
      "$uid": {
        ".read": "auth.uid === $uid || root.child('games').child($uid).exists()",
        ".write": "auth.uid === $uid"
      }
    },
    "games": {
      "$gameId": {
        ".read": true,
        ".write": "!data.exists() || data.child('host').val() === auth.uid"
      }
    },
    "scores": {
      "$uid": {
        ".read": true,
        ".write": "auth.uid === $uid"
      }
    }
  }
}
```

### Step 7: Deploy Firebase (Optional)

If you want to host on Firebase Hosting:

```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy
```

## Deployment to GitHub Pages

### Step 1: Build Your Project

```bash
npm run build
```

This creates a `dist` directory with your production-ready files.

### Step 2: Configure GitHub Pages

1. Go to your repository on GitHub
2. Navigate to **Settings** → **Pages**
3. Under **"Source"**, select **"Deploy from a branch"** or **"GitHub Actions"**
4. If using a branch, select `main` (or `gh-pages` if preferred) and `/root` folder
5. Click **"Save"**

### Step 3: Automated Deployment with GitHub Actions

Create `.github/workflows/deploy.yml`:

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    
    steps:
    - uses: actions/checkout@v3
    
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '18'
        cache: 'npm'
    
    - name: Install dependencies
      run: npm install
    
    - name: Build
      run: npm run build
      env:
        VITE_FIREBASE_API_KEY: ${{ secrets.FIREBASE_API_KEY }}
        VITE_FIREBASE_AUTH_DOMAIN: ${{ secrets.FIREBASE_AUTH_DOMAIN }}
        VITE_FIREBASE_PROJECT_ID: ${{ secrets.FIREBASE_PROJECT_ID }}
        VITE_FIREBASE_STORAGE_BUCKET: ${{ secrets.FIREBASE_STORAGE_BUCKET }}
        VITE_FIREBASE_MESSAGING_SENDER_ID: ${{ secrets.FIREBASE_MESSAGING_SENDER_ID }}
        VITE_FIREBASE_APP_ID: ${{ secrets.FIREBASE_APP_ID }}
        VITE_FIREBASE_DATABASE_URL: ${{ secrets.FIREBASE_DATABASE_URL }}
    
    - name: Deploy to GitHub Pages
      uses: peaceiris/actions-gh-pages@v3
      with:
        github_token: ${{ secrets.GITHUB_TOKEN }}
        publish_dir: ./dist
```

### Step 4: Add GitHub Secrets

1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Click **"New repository secret"**
3. Add each Firebase configuration variable:
   - `FIREBASE_API_KEY`
   - `FIREBASE_AUTH_DOMAIN`
   - `FIREBASE_PROJECT_ID`
   - `FIREBASE_STORAGE_BUCKET`
   - `FIREBASE_MESSAGING_SENDER_ID`
   - `FIREBASE_APP_ID`
   - `FIREBASE_DATABASE_URL`

### Step 5: Manual Deployment (Optional)

If you prefer manual deployment:

```bash
# Install gh-pages package
npm install --save-dev gh-pages

# Add to package.json scripts:
# "deploy": "npm run build && gh-pages -d dist"

npm run deploy
```

### Step 6: Access Your Game

Your game will be available at:
```
https://thangnguyenmcs-oss.github.io/bang-dong-nhau
```

## Playing the Game

### How to Play

1. **Start the Game**: Visit the deployed URL or run locally with `npm start`
2. **Create Account**: Sign up with email or use anonymous login
3. **Join or Create Game**: 
   - Create a new game session
   - Or join an existing game using a game code
4. **Gameplay**: Follow on-screen instructions to play
5. **Multiplayer**: Invite friends to join your game session

### Controls

(Add specific control instructions for your game here)

### Tips & Tricks

(Add gameplay tips here)

## Environment Variables

Create a `.env` file with the following variables:

```env
# Firebase Configuration
VITE_FIREBASE_API_KEY=your_api_key
VITE_FIREBASE_AUTH_DOMAIN=your_auth_domain
VITE_FIREBASE_PROJECT_ID=your_project_id
VITE_FIREBASE_STORAGE_BUCKET=your_storage_bucket
VITE_FIREBASE_MESSAGING_SENDER_ID=your_messaging_sender_id
VITE_FIREBASE_APP_ID=your_app_id
VITE_FIREBASE_DATABASE_URL=your_database_url

# Application
VITE_APP_NAME=Bang Dong Nhau
VITE_APP_ENV=development
```

## Project Structure

```
bang-dong-nhau/
├── public/              # Static assets
├── src/
│   ├── components/      # Reusable components
│   ├── pages/           # Page components
│   ├── utils/           # Utility functions
│   ├── firebase/        # Firebase configuration
│   ├── styles/          # CSS stylesheets
│   └── main.js          # Entry point
├── dist/                # Build output (generated)
├── .env.example         # Environment variables template
├── .github/
│   └── workflows/       # GitHub Actions workflows
├── package.json         # Project dependencies
├── vite.config.js       # Vite configuration (if using Vite)
└── README.md            # This file
```

## Troubleshooting

### Common Issues

**Issue**: Firebase connection errors
- **Solution**: Verify your `.env` file contains correct Firebase credentials
- Check Firebase security rules allow your authentication method

**Issue**: GitHub Pages shows blank page
- **Solution**: Ensure `npm run build` completes successfully
- Check GitHub Actions workflow logs for build errors
- Verify deploy directory is correct in workflow

**Issue**: Real-time updates not working
- **Solution**: Check Firebase Realtime Database rules
- Ensure user is authenticated (if required by rules)
- Check browser console for error messages

**Issue**: Port already in use locally
- **Solution**: Kill the process using the port or specify a different port
```bash
npm start -- --port 3001
```

## Development Workflow

1. Create a feature branch: `git checkout -b feature/your-feature`
2. Make your changes
3. Test locally: `npm start`
4. Build for production: `npm run build`
5. Commit changes: `git commit -am 'Add your message'`
6. Push to GitHub: `git push origin feature/your-feature`
7. Create a Pull Request
8. After review and merge, GitHub Actions will automatically deploy

## Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## Performance Tips

- Use code splitting for large applications
- Optimize images and assets
- Implement lazy loading for components
- Monitor Firebase usage and optimize queries
- Use production-grade Firebase security rules

## Security Best Practices

- Never commit `.env` files with real credentials
- Use GitHub Secrets for sensitive data
- Implement proper Firebase security rules
- Validate all user inputs
- Use HTTPS for all communications
- Regularly update dependencies: `npm update`

## Resources

- [Firebase Documentation](https://firebase.google.com/docs)
- [GitHub Pages Guide](https://pages.github.com/)
- [Web Development Best Practices](https://developer.mozilla.org/)
- [Firebase Security Best Practices](https://firebase.google.com/docs/database/usage/best-practices)

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

For support, please:

1. Check the [Troubleshooting](#troubleshooting) section
2. Create an [Issue](https://github.com/thangnguyenmcs-oss/bang-dong-nhau/issues) on GitHub
3. Contact the project maintainers

## Authors

- **Thang Nguyen** - Project Creator and Maintainer

---

**Last Updated**: December 27, 2025

Enjoy the game! ����✨
