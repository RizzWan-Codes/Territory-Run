# 🏃‍♂️ Territory Run

**Turn Your City Into Your Playground - GPS-Powered Territory Capture Game**

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Firebase](https://img.shields.io/badge/Firebase-FFCA28?logo=firebase&logoColor=black)](https://firebase.google.com/)
[![Mappls](https://img.shields.io/badge/Mappls-00A550?logo=map&logoColor=white)](https://apis.mappls.com/)
[![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?logo=javascript&logoColor=black)](https://developer.mozilla.org/en-US/docs/Web/JavaScript)

> **Territory Run** is a location-based fitness game that gamifies running by letting you capture real-world territory through GPS-tracked runs. Create loops, capture areas, earn coins, and compete on global leaderboards!

---

## 📸 Screenshots

[Add screenshots here]

| Dashboard | Active Run | Territory Captured |
|-----------|------------|-------------------|
| ![Dashboard](link) | ![Run](link) | ![Captured](link) |

---

## 🎮 Features

### Core Gameplay
- 🗺️ **Real-time GPS Tracking** - See your live position on interactive maps
- 🏃 **Run Tracking** - Record your path with a beautiful green trail
- 🔄 **Loop Detection** - Close loops to capture territory
- 🎯 **Territory Capture** - Turn enclosed areas into your conquests
- 💰 **Coin System** - Earn rewards based on captured area
- 🏆 **Live Leaderboard** - Compete with players worldwide

### Technical Features
- ✅ High-accuracy GPS with position smoothing
- ✅ Responsive mobile-first design
- ✅ Real-time data synchronization
- ✅ Offline-capable progressive web app
- ✅ Indian map coverage via Mappls
- ✅ Firebase authentication & database
- ✅ Dark theme optimized for outdoor use

---

## 🚀 Quick Start

### Prerequisites
- Modern web browser (Chrome 90+, Safari 14+, Firefox 88+)
- GPS-enabled device
- Internet connection
- Firebase account
- Mappls API key

### Installation

1. **Clone the repository**
```bash
git clone https://github.com/yourusername/territory-run.git
cd territory-run
```

2. **Set up Firebase**
   - Create a project at [Firebase Console](https://console.firebase.google.com/)
   - Enable Authentication (Email/Password & Google)
   - Create a Firestore database
   - Copy your Firebase config

3. **Configure Firebase**

Edit the Firebase configuration in your HTML files:

```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT.firebasestorage.app",
    messagingSenderId: "YOUR_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

4. **Get Mappls API Key**
   - Register at [Mappls APIs](https://apis.mappls.com/)
   - Create a new project
   - Copy your API key
   - Replace in the script tag:

```html
<script src="https://apis.mappls.com/advancedmaps/api/YOUR_API_KEY/map_sdk?layer=vector&v=3.0&callback=initMap1"></script>
```

5. **Set up Firestore Security Rules**

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read: if true;
      allow write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

6. **Deploy**

**Option A - Local Development:**
```bash
# Simple HTTP server
python -m http.server 8000
# or
npx serve
```

**Option B - Firebase Hosting:**
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
firebase deploy
```

**Option C - Other Hosting:**
- Netlify: Drag & drop folder
- Vercel: Connect GitHub repo
- GitHub Pages: Push to gh-pages branch

---

## 📂 Project Structure

```
territory-run/
├── index.html              # Landing/Login page
├── login.html             # Authentication page
├── dashboard.html         # Main game interface
├── README.md              # This file
├── assets/                # Images, icons, etc.
│   ├── logo.png
│   └── screenshots/
└── docs/                  # Additional documentation
    └── CONTRIBUTING.md
```

---

## 🎯 How to Play

### 1. Sign Up / Login
- Create an account or login with Google
- Your profile is created automatically

### 2. Start Your Run
- Click **"Start Run"** button at the bottom
- Begin moving - your path will be tracked with a green line

### 3. Create a Loop
- Run in any pattern (circle, square, triangle, etc.)
- The goal is to return to your starting point

### 4. Close the Loop
- Get within **15 meters** of your starting point
- Watch the progress bar turn green
- Click **"Stop Run"** to capture

### 5. Earn Rewards
- **Area captured** = Your territory (in m²)
- **Coins earned** = 1 coin per 100 m² + 10 bonus coins
- **Leaderboard rank** = Based on total area

### 6. Compete
- Check the leaderboard to see top players
- Capture more territory to climb the ranks!

---

## ⚙️ Game Mechanics

| Mechanic | Value | Description |
|----------|-------|-------------|
| Safe Zone Radius | 20m | Starting area indicator |
| Loop Closure Threshold | 15m | Distance to close loop |
| Minimum Capture Area | 100 m² | Smallest valid territory |
| Coins Per 100 m² | 1 coin | Base reward rate |
| Bonus Coins | 10 coins | Per successful run |

---

## 🛠️ Technology Stack

### Frontend
- **HTML5/CSS3** - Structure and styling
- **JavaScript (ES6+)** - Game logic and interactions
- **Tailwind CSS** - Utility-first styling framework

### Mapping & Geolocation
- **Mappls SDK v3.0** - Interactive maps (Indian coverage)
- **Geolocation API** - High-accuracy GPS tracking
- **Haversine Formula** - Distance calculations
- **Shoelace Algorithm** - Polygon area calculations

### Backend & Database
- **Firebase Authentication** - User management
- **Cloud Firestore** - NoSQL database
- **Real-time Listeners** - Live leaderboard updates

### Additional APIs
- **DiceBear API** - Auto-generated avatars

---

## 🧮 Algorithms & Calculations

### Distance Calculation (Haversine)
```javascript
function calculateDistance(lat1, lon1, lat2, lon2) {
    const R = 6371000; // Earth's radius in meters
    const dLat = toRad(lat2 - lat1);
    const dLon = toRad(lon2 - lon1);
    const a = Math.sin(dLat/2) * Math.sin(dLat/2) +
              Math.cos(toRad(lat1)) * Math.cos(toRad(lat2)) *
              Math.sin(dLon/2) * Math.sin(dLon/2);
    const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1-a));
    return R * c;
}
```

### Area Calculation (Shoelace)
```javascript
function calculatePolygonArea(coordinates) {
    let area = 0;
    for (let i = 0; i < coordinates.length; i++) {
        const j = (i + 1) % coordinates.length;
        const xi = coordinates[i].lng * 111320 * Math.cos(toRad(coordinates[i].lat));
        const yi = coordinates[i].lat * 110540;
        const xj = coordinates[j].lng * 111320 * Math.cos(toRad(coordinates[j].lat));
        const yj = coordinates[j].lat * 110540;
        area += xi * yj - xj * yi;
    }
    return Math.abs(area / 2);
}
```

---

## 📊 Database Schema

### Users Collection (`users/{userId}`)
```javascript
{
  gamertag: "Runner123456",
  email: "user@example.com",
  avatarUrl: "https://api.dicebear.com/...",
  coins: 0,
  totalArea: 0,
  territoriesCaptured: 0,
  level: 1,
  xp: 0,
  createdAt: "2026-01-03T...",
  lastActive: "2026-01-03T..."
}
```

---

## 🚧 Known Issues & Limitations

### Current Limitations
- ⚠️ GPS accuracy varies (5-20m) based on device and environment
- ⚠️ Battery drain during extended runs (GPS intensive)
- ⚠️ Requires internet connection (no offline mode yet)
- ⚠️ Mappls coverage limited to India
- ⚠️ No territory conflicts (players capture independently)

### Workarounds
- Use high-accuracy GPS mode for best results
- Keep phone charged during long runs
- Test in open areas with clear sky view
- Wait for GPS accuracy <20m before starting runs

---

## 🗺️ Roadmap

### Version 1.1 (Next Release)
- [ ] Progressive Web App (PWA) support
- [ ] Offline mode with sync
- [ ] Enhanced run statistics
- [ ] Friend system
- [ ] Achievement badges

### Version 2.0 (Future)
- [ ] Territory interactions (attack/defend)
- [ ] Multiplayer live runs
- [ ] AR mode
- [ ] Wearable integration (Apple Watch, Fitbit)
- [ ] Native mobile apps

### Version 3.0 (Long-term)
- [ ] AI-powered route suggestions
- [ ] Global expansion (beyond India)
- [ ] Monetization (premium features)
- [ ] Community events & tournaments

See [ROADMAP.md](docs/ROADMAP.md) for detailed plans.

---

## 🤝 Contributing

We welcome contributions! Here's how you can help:

### Ways to Contribute
- 🐛 Report bugs via [GitHub Issues](https://github.com/yourusername/territory-run/issues)
- 💡 Suggest features or improvements
- 🔧 Submit pull requests
- 📖 Improve documentation
- 🧪 Test on different devices

### Development Setup
1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Make your changes
4. Test thoroughly
5. Commit (`git commit -m 'Add amazing feature'`)
6. Push (`git push origin feature/amazing-feature`)
7. Open a Pull Request

See [CONTRIBUTING.md](docs/CONTRIBUTING.md) for detailed guidelines.

---

## 🐛 Troubleshooting

### Map shows black screen
**Solution:** Check Mappls API key is correct and domain is whitelisted

### GPS not working
**Solution:** 
- Enable location permissions in browser
- Ensure GPS is enabled on device
- Try in open area (outdoor)
- Check browser console for errors

### Authentication fails
**Solution:**
- Verify Firebase config is correct
- Check Firebase Authentication is enabled
- Ensure authorized domains are configured

### Leaderboard not updating
**Solution:**
- Check Firestore security rules allow reads
- Verify internet connection
- Check browser console for errors

### Territory not capturing
**Solution:**
- Ensure loop is closed (within 15m of start)
- Check minimum area requirement (100 m²)
- Verify GPS accuracy is reasonable (<50m)

---

## 📱 Browser Compatibility

| Browser | Desktop | Mobile | Notes |
|---------|---------|--------|-------|
| Chrome | ✅ | ✅ | Recommended |
| Safari | ✅ | ✅ | iOS 14+ |
| Firefox | ✅ | ✅ | |
| Edge | ✅ | ✅ | Chromium-based |
| Opera | ✅ | ⚠️ | Limited testing |

---

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

```
MIT License

Copyright (c) 2026 Territory Run

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```

---

## 👥 Team / Credits

**Created by:** [Your Team Name]

**Team Members:**
- [Your Name] - Lead Developer - [GitHub](https://github.com/yourusername) | [LinkedIn](https://linkedin.com/in/yourprofile)
- [Team Member 2] - Role
- [Team Member 3] - Role

**Special Thanks:**
- Mappls (MapmyIndia) for excellent Indian map coverage
- Firebase team for seamless backend infrastructure
- Tailwind CSS for beautiful UI components
- [Hackathon Name] organizers

---

## 🌟 Show Your Support

If you like this project, please give it a ⭐ on GitHub!

### Share on Social Media
- 🐦 [Tweet about Territory Run](https://twitter.com/intent/tweet?text=Check%20out%20Territory%20Run%20-%20A%20GPS-powered%20fitness%20game!%20https://github.com/yourusername/territory-run)
- 📘 [Share on Facebook](https://www.facebook.com/sharer/sharer.php?u=https://github.com/yourusername/territory-run)
- 💼 [Share on LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https://github.com/yourusername/territory-run)

---

## 📞 Contact

**Project Link:** [https://github.com/yourusername/territory-run](https://github.com/yourusername/territory-run)

**Live Demo:** [https://territory-run.web.app](https://territory-run.web.app)

**Email:** your.email@example.com

**Discord:** [Join our community](https://discord.gg/yourserver)

---

## 🔗 Related Projects

- [Zombies, Run!](https://zombiesrungame.com/) - Narrative-driven running app
- [Pokémon GO](https://pokemongolive.com/) - AR location-based game
- [Ingress](https://ingress.com/) - Location-based MMO
- [Strava](https://www.strava.com/) - Social fitness tracking

---

## 📚 Additional Resources

### Documentation
- [Mappls API Documentation](https://www.mappls.com/api/)
- [Firebase Documentation](https://firebase.google.com/docs)
- [Geolocation API Specification](https://w3c.github.io/geolocation-api/)

### Tutorials
- [Building Location-Based Apps](https://developers.google.com/web/fundamentals/native-hardware/user-location)
- [Firebase Real-time Database](https://firebase.google.com/docs/database)
- [GPS Tracking Best Practices](https://developer.android.com/guide/topics/location/strategies)

### Research Papers
- ["Gamification of Exercise" - Journal of Medical Internet Research](https://www.jmir.org/)
- ["Location-Based Gaming Impact on Physical Activity" - Health Psychology Review](https://www.tandfonline.com/toc/rhpr20/current)

---

## 🎉 Acknowledgments

Built with ❤️ for [Hackathon Name]

**Technologies Used:**
- [Firebase](https://firebase.google.com/)
- [Mappls](https://apis.mappls.com/)
- [Tailwind CSS](https://tailwindcss.com/)
- [DiceBear](https://dicebear.com/)

**Inspired by:**
- The need for engaging fitness solutions
- India's growing health consciousness
- The success of location-based gaming

---

<div align="center">

**"Every run is a conquest. Every neighborhood is a battlefield. Your city is your playground."**

Made with 🏃‍♂️ by the Territory Run Team

[⬆ Back to top](#-territory-run)

</div>
