# 🌿 SmartEco Rwanda

**Smart Waste Management with Rewards**

A Progressive Web Application (PWA) for smart waste collection in Rwanda. Schedule pickups, track collectors in real-time, earn EcoPoints, and access via mobile app, USSD, or WhatsApp.

![SmartEco Banner](https://img.shields.io/badge/SmartEco-Rwanda-22C55E?style=for-the-badge&logo=recycle&logoColor=white)
![PWA](https://img.shields.io/badge/PWA-Ready-5A0FC8?style=for-the-badge&logo=pwa&logoColor=white)
![Mobile](https://img.shields.io/badge/Mobile-Friendly-3DDC84?style=for-the-badge&logo=android&logoColor=white)

---

## 🚀 Live Demo

**[Try SmartEco Now →](https://YOUR-USERNAME.github.io/smarteco-rwanda/)**

---

## 📱 Features

### Multi-Channel Access
| Channel | Description | Status |
|---------|-------------|--------|
| 📱 **Mobile App** | Full-featured PWA with live tracking | ✅ Complete |
| 📞 **USSD** | Works on any phone - *384*22# | ✅ Simulator |
| 💬 **WhatsApp** | Chat bot for scheduling | ✅ Simulator |

### Core Features
- ✅ User Registration & Authentication
- ✅ Schedule Waste Pickups
- ✅ Live Collector Tracking (GPS simulation)
- ✅ QR Code Bin Scanning
- ✅ EcoPoints Rewards System
- ✅ Mobile Money Payment (MTN MoMo, Airtel)
- ✅ Environmental Impact Dashboard
- ✅ Waste Sorting Guide
- ✅ Referral Program
- ✅ Subscription Management
- ✅ Push Notifications
- ✅ Offline Support

---

## 🛠️ Technology Stack

| Layer | Technology |
|-------|------------|
| Frontend | HTML5, CSS3, JavaScript (ES6+) |
| Data Storage | localStorage API |
| PWA | Service Worker, Web App Manifest |
| Styling | Custom CSS with CSS Variables |
| Fonts | Google Fonts (Plus Jakarta Sans, Space Grotesk) |

---

## 📂 Project Structure

```
smarteco-rwanda/
├── index.html          # Main application (single-file PWA)
├── manifest.json       # PWA manifest for installation
├── sw.js              # Service worker for offline support
├── DATABASE_SCHEMA.md  # Data storage documentation
├── DEPLOYMENT_GUIDE.md # Technical audit & deployment guide
└── README.md          # This file
```

---

## 🚀 Quick Start

### Option 1: View Online
Simply visit the [live demo](https://YOUR-USERNAME.github.io/smarteco-rwanda/)

### Option 2: Run Locally
```bash
# Clone the repository
git clone https://github.com/YOUR-USERNAME/smarteco-rwanda.git

# Navigate to folder
cd smarteco-rwanda

# Open in browser (use any local server)
# Option A: Python
python -m http.server 8000

# Option B: Node.js
npx serve

# Option C: Just open index.html in browser
```

### Option 3: Install as App
1. Open the app in Chrome/Safari
2. Click "Add to Home Screen" or install prompt
3. Use like a native app!

---

## 📱 Screenshots

| Welcome | Home | USSD | WhatsApp |
|---------|------|------|----------|
| Splash & Access Options | Dashboard & Tracking | Phone Simulator | Chat Bot |

---

## 💾 Data Storage

The app uses a client-side database with these collections:

- `users` - User profiles & subscriptions
- `bins` - Smart bin registry
- `pickups` - Scheduled & completed pickups
- `transactions` - Payments & redemptions
- `points_history` - EcoPoints ledger
- `notifications` - User alerts
- `ussd_sessions` - USSD interaction logs
- `whatsapp_sessions` - Chat conversation logs

See [DATABASE_SCHEMA.md](DATABASE_SCHEMA.md) for full documentation.

---

## 🔧 Configuration

### PWA Manifest
Edit `manifest.json` to customize:
- App name & description
- Theme colors
- Icons

### Service Worker
Edit `sw.js` to modify:
- Cache strategy
- Cached resources

---

## 🌍 Deployment

### GitHub Pages (Free)
1. Fork this repository
2. Go to Settings → Pages
3. Select `main` branch
4. Your app is live at `https://YOUR-USERNAME.github.io/smarteco-rwanda/`

### Other Options
- **Vercel**: Connect GitHub repo for instant deploy
- **Netlify**: Drag & drop deployment
- **Firebase Hosting**: `firebase deploy`

See [DEPLOYMENT_GUIDE.md](DEPLOYMENT_GUIDE.md) for detailed instructions.

---

## 🗺️ Roadmap

### Current Version (v1.0) - Demo/MVP
- [x] Complete UI/UX
- [x] All user flows
- [x] Client-side data storage
- [x] PWA functionality

### Future Versions
- [ ] **v2.0** - Real backend (Node.js + MongoDB)
- [ ] **v2.1** - MTN MoMo & Airtel Money integration
- [ ] **v2.2** - Africa's Talking USSD integration
- [ ] **v2.3** - WhatsApp Business API
- [ ] **v3.0** - Native mobile apps (React Native)

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 👥 Team

**SmartEco Rwanda**
- Turning Waste into Wealth 🌿

---

## 📞 Contact

- **Website**: [smarteco.rw](https://smarteco.rw)
- **Email**: info@smarteco.rw
- **WhatsApp**: +250 788 000 000

---

## ⭐ Star This Repo

If you find this project useful, please give it a star! ⭐

---

*Made with ♻️ in Rwanda*
