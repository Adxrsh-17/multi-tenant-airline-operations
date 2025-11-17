# SkyFleet Airlines ✈️

A comprehensive airline management system featuring role-based dashboards for administrators, crew members, and passengers. Built with Firebase and vanilla JavaScript for real-time operations management.

![Firebase](https://img.shields.io/badge/Firebase-9.22.0-orange.svg)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6+-yellow.svg)

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Tech Stack](#tech-stack)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [User Roles](#user-roles)
- [Screenshots](#screenshots)
- [Contributing](#contributing)

## 🎯 Overview

SkyFleet is a full-stack web application that streamlines airline operations through three specialized portals. The system handles everything from flight scheduling and crew management to passenger bookings and seat selection.

**Live Demo:** [Insert your demo link here]

## ✨ Features

### Admin Dashboard
- ✅ Flight management (create, edit, delete, schedule)
- ✅ Fleet control with real-time status tracking
- ✅ Crew roster management and assignment
- ✅ Smart crew scheduling with conflict detection
- ✅ Visual calendar for crew assignments
- ✅ Booking oversight and analytics
- ✅ Comprehensive analytics dashboard

### Crew Portal
- ✅ Personal flight schedule view
- ✅ Assignment notifications
- ✅ Detailed flight information
- ✅ Working hours tracking
- ✅ Role-specific duty roster
- ✅ Profile management

### Passenger Portal
- ✅ Advanced flight search with filters
- ✅ Interactive seat selection (Economy, Business, First Class)
- ✅ Real-time booking system
- ✅ Loyalty points program
- ✅ Booking history and management
- ✅ Notifications and alerts

## 🛠️ Tech Stack

**Frontend:**
- HTML5, CSS3, JavaScript (ES6+)
- TailwindCSS for styling
- SweetAlert2 for modals
- Chart.js for analytics

**Backend:**
- Firebase Authentication
- Cloud Firestore (NoSQL database)
- Firebase Hosting

**Tools:**
- Git for version control
- Firebase CLI for deployment

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

- Node.js (v14.0.0 or higher)
- npm or yarn package manager
- Git
- A Firebase account (free tier works)
- Modern web browser (Chrome, Firefox, Safari, Edge)

## 🚀 Installation

### 1. Clone the Repository
```bash
git clone https://github.com/yourusername/skyfleet-airlines.git
cd skyfleet-airlines
```

### 2. Firebase Setup

1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Create a new project named "skyfleet-app"
3. Enable Authentication (Email/Password method)
4. Create a Firestore Database (Start in production mode)
5. Get your Firebase configuration

### 3. Configure Firebase

Replace the Firebase configuration in all relevant files:

**Files to update:**
- `admin-dashboard.html`
- `crew-dashboard.html`
- `user-dashboard.html`
- `login.html`
- `signup.html`
```javascript
const firebaseConfig = {
    apiKey: "YOUR_API_KEY",
    authDomain: "YOUR_PROJECT_ID.firebaseapp.com",
    projectId: "YOUR_PROJECT_ID",
    storageBucket: "YOUR_PROJECT_ID.appspot.com",
    messagingSenderId: "YOUR_MESSAGING_SENDER_ID",
    appId: "YOUR_APP_ID"
};
```

### 4. Firestore Security Rules

Set up Firestore security rules in Firebase Console:
```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
      allow read: if request.auth != null && get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
    }
    
    match /flights/{flightId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
    }
    
    match /fleet/{aircraftId} {
      allow read: if request.auth != null;
      allow write: if request.auth != null && get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin';
    }
    
    match /bookings/{bookingId} {
      allow read: if request.auth != null && (resource.data.userId == request.auth.uid || get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin');
      allow create: if request.auth != null;
      allow update, delete: if request.auth != null && (resource.data.userId == request.auth.uid || get(/databases/$(database)/documents/users/$(request.auth.uid)).data.role == 'admin');
    }
    
    match /notifications/{notificationId} {
      allow read, write: if request.auth != null && resource.data.userId == request.auth.uid;
    }
  }
}
```

### 5. Deploy Locally

Simply open `index.html` in your web browser, or use a local server:
```bash
# Using Python
python -m http.server 8000

# Using Node.js http-server
npx http-server -p 8000

# Using PHP
php -S localhost:8000
```

Visit `http://localhost:8000` in your browser.

## ⚙️ Configuration

### Creating Test Accounts

1. **Admin Account:**
   - Go to signup page
   - Use email: `admin@skyfleet.com`
   - Password: `admin123`
   - Select "Admin" role

2. **Crew Account:**
   - Email: `crew@skyfleet.com`
   - Password: `crew123`
   - Select "Crew" role

3. **Passenger Account:**
   - Email: `passenger@skyfleet.com`
   - Password: `pass123`
   - Select "Passenger" role

### Adding Sample Data

After creating an admin account, add:

1. **Fleet:** Navigate to Fleet Control → Add Aircraft
2. **Flights:** Go to Flight Management → Add New Flight
3. **Crew:** Add crew members in Crew Roster section

## 📖 Usage

### For Administrators
```bash
1. Login with admin credentials
2. Dashboard shows overview statistics
3. Add flights via Flight Management
4. Assign crew using Crew Scheduling calendar
5. Monitor bookings and analytics
```

### For Crew Members
```bash
1. Login with crew credentials
2. View assigned flights on dashboard
3. Check detailed flight information
4. Access personal schedule
5. Update availability status
```

### For Passengers
```bash
1. Login or signup as passenger
2. Search for flights using booking form
3. Select preferred flight
4. Choose seat class and specific seats
5. Complete booking and earn loyalty points
```

## 📁 Project Structure
```
skyfleet-airlines/
├── index.html                  # Landing page
├── login.html                  # Login page
├── signup.html                 # Signup page
├── admin-dashboard.html        # Admin portal
├── crew-dashboard.html         # Crew portal
├── user-dashboard.html         # Passenger portal
├── README.md                   # Documentation
├── LICENSE                     # MIT License
└── assets/                     # Images and assets (if any)
```

## 👥 User Roles

| Role | Access Level | Capabilities |
|------|-------------|--------------|
| **Admin** | Full Access | Manage flights, fleet, crew, view all bookings, analytics |
| **Crew** | Limited | View assignments, personal schedule, flight details |
| **Passenger** | Standard | Book flights, select seats, view bookings, earn points |

## 🔐 Security Features

- Firebase Authentication with email/password
- Role-based access control (RBAC)
- Firestore security rules
- Client-side validation
- Secure session management

## 🌐 Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## 🐛 Known Issues

- Seat map generation uses random occupied seats (demo purposes)
- No payment gateway integration (simulation only)
- Email notifications not implemented (uses in-app notifications)

## 🔮 Future Enhancements

- [ ] Real payment gateway integration (Stripe/PayPal)
- [ ] Email notification system
- [ ] Mobile responsive improvements
- [ ] Multi-language support
- [ ] Advanced analytics with charts
- [ ] PDF ticket generation
- [ ] Flight delay notifications
- [ ] Check-in system





## 🙏 Acknowledgments

- Firebase for backend infrastructure
- TailwindCSS for styling framework
- SweetAlert2 for beautiful alerts
- Chart.js for data visualization
- Font Awesome for icons


## 📊 Project Status

🟢 Active Development
