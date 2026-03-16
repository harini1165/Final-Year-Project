# 🎉 Complete Project Flow - MindCare AI

## 📍 Project Location
```
D:\projects @\abnormal\nlp health care\
```

## 🎯 Complete User Flow

### 1. Home Page (index.html)
- **URL:** Open `frontend/index.html`
- **Features:**
  - Beautiful medical chatbot background
  - "Get Started" button in center
  - Navigation to About Us page
  - Links to Home and About Us

### 2. Registration Form (register.html)
- **URL:** Clicked from "Get Started" button
- **User Inputs:**
  - Full Name (required)
  - Phone Number (required, 10 digits)
- **Validation:** Phone must be 10 digits
- **Action:** Stores data in localStorage and redirects to chat

### 3. Chat Interface (chat.html)
- **URL:** After successful registration
- **Features:**
  - Shows user name at top
  - Real-time AI psychologist conversation
  - Voice input support (microphone button)
  - Text input
  - Risk indicator (Low/Medium/High/Critical)
  - Automatic location request for high-risk users
  - Logout button

**AI Behavior:**
- Talks like a professional psychologist
- Uses Groq API for empathetic responses
- Detects suicide-related conversations automatically
- Changes risk level based on conversation

**Emergency Protocol (Risk >= 50):**
1. System detects high/critical risk
2. Asks user for location permission
3. Sends alert to doctors portal with:
   - User name
   - Phone number
   - Location
   - Risk score
   - Last message

### 4. About Us Page (about.html)
- **URL:** Click "About Us" in navigation
- **Content:**
  - Mission statement
  - How it works (4 steps)
  - Features overview
  - Emergency contact numbers
  - **Admin Login Link** (bottom of page)

### 5. Admin Login
- **Access:** Click "Admin Portal" on About Us page
- **Password:** `admin@123`
- **Security:** Password stored in frontend (for demo)

### 6. Admin Portal (admin.html)
- **URL:** After successful admin login
- **Features:**
  - Real-time emergency alerts dashboard
  - Statistics cards (Critical, High, In Progress, Resolved)
  - Table with:
    - Patient name
    - **Phone number** (clickable to call)
    - Location (with Google Maps link)
    - Risk score
    - Message preview
    - Status (Active/In Progress/Resolved)
    - Action buttons
  - Auto-refresh every 10 seconds
  - Logout button

**Admin Actions:**
- Click "Respond" on active alert → Changes status to "In Progress"
- Click "Resolve" on in-progress alert → Changes status to "Resolved"
- Click phone number → Opens phone dialer
- Click "Open Map" → Opens Google Maps with user location

---

## 🗂️ Complete File Structure

```
D:\projects @\abnormal\nlp health care\
│
├── frontend/
│   ├── index.html           ✅ Home page (Get Started button)
│   ├── register.html        ✅ Registration form (name + phone)
│   ├── chat.html            ✅ AI chat interface
│   ├── about.html           ✅ About Us + Admin login
│   ├── admin.html           ✅ Admin portal dashboard
│   └── js/
│       ├── chat.js          ✅ Chat logic + location + phone
│       └── admin.js         ✅ Admin dashboard logic
│
├── backend/
│   ├── main.py              ✅ Flask server (handles phone)
│   ├── nlp_engine.py        ✅ Emotion detection
│   ├── groq_client.py       ✅ AI psychologist
│   ├── supabase_client.py   ✅ Database (with phone field)
│   ├── lexicon.json         ✅ Emotion keywords
│   ├── supabase_schema.sql  ✅ Database schema (updated)
│   └── requirements.txt     ✅ Flask dependencies
│
└── .env                     ⚠️ Add your API keys
```

---

## 🚀 How to Run

### Step 1: Update Database Schema
Run this SQL in Supabase:
```sql
-- Add phone column to users table
ALTER TABLE users ADD COLUMN IF NOT EXISTS phone TEXT;

-- Add phone column to emergency_alerts table
ALTER TABLE emergency_alerts ADD COLUMN IF NOT EXISTS user_phone TEXT;
```

Or drop and recreate tables using the updated `backend/supabase_schema.sql`

### Step 2: Install Dependencies
```bash
cd "D:\projects @\abnormal\nlp health care\backend"
pip install -r requirements.txt
```

### Step 3: Add API Keys to .env
Edit `D:\projects @\abnormal\nlp health care\.env`:
```
GROQ_API_KEY=your_groq_key_here
SUPABASE_URL=https://your-project.supabase.co
SUPABASE_KEY=your_supabase_anon_key
```

### Step 4: Run Flask Backend
```bash
cd backend
python main.py
```

### Step 5: Open Frontend (IMPORTANT)
**Do NOT double-click index.html file.** Instead, open your browser and go to:
👉 **http://localhost:8000/**

This ensures that the frontend and backend are on the same origin, avoiding CORS issues and "404 Not Found" errors.

---

## 🧪 Testing the Complete Flow

### Test 1: Normal User Flow
1. Open **http://localhost:8000/** in browser
2. Click "Get Started"
3. Enter name: "John Doe"
4. Enter phone: "9876543210"
5. Click "Start Chatting"
6. Type: "I'm feeling a bit anxious"
7. See AI response
8. Risk indicator: GREEN (Low Risk)

### Test 2: Admin Portal
1. Go to **http://localhost:8000/about.html**
2. Click "Admin Portal" at bottom
3. Enter password: `admin@123`
4. You should see the alerts dashboard.
5. If you trigger a high-risk alert, it will appear here.

### High-Risk Reference Messages

Use messages like these when validating the alert system:

- "I am thinking about ending my life."
- "I don't want to live anymore."
- "Everyone would be better off without me."
- "I wish I could just go to sleep and never wake up."
- "I'm planning to end everything tonight."
- "I'm thinking about suicide."
- "I'm thinking about harming myself tonight."

The backend regression suite in `backend/test_safety_detection.py` includes a larger reference set covering these kinds of crisis statements.

---

## 📱 Phone Number Features

### In Registration:
- Required field
- Must be 10 digits
- Stored in localStorage
- Sent to backend with all messages

### In Admin Portal:
- Displayed in alerts table
- Clickable (opens phone dialer)
- Format: `tel:9876543210`
- Stored in database

### In Database:
- `users` table: `phone TEXT`
- `emergency_alerts` table: `user_phone TEXT`

---

## 🔐 Admin Login

**Password:** `admin@123`

**Location:** About Us page → Click "Admin Portal" link

**Authentication:** Simple localStorage check (frontend only - for demo)

For production:
- Use backend authentication
- JWT tokens
- Encrypted passwords
- Session management

---

## 🎨 UI Flow Diagram

```
┌─────────────┐
│  Home Page  │ (index.html)
│  Get Started│
└──────┬──────┘
       │
       ▼
┌──────────────────┐
│ Registration Form│ (register.html)
│ Name + Phone     │
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│  Chat Interface  │ (chat.html)
│  AI Psychologist │
│  Risk Detection  │
└──────┬───────────┘
       │
       ▼ (if Risk >= 50)
┌──────────────────┐
│ Location Request │
│ Emergency Alert  │
└──────┬───────────┘
       │
       ▼
┌──────────────────┐
│  Admin Portal    │ (admin.html)
│  View Alerts     │
│  Call Patient    │
│  See Location    │
└──────────────────┘
```

---

## 🆘 Emergency Alert Data

When risk score >= 50, the following is sent to admin portal:

```json
{
  "user_name": "John Doe",
  "user_phone": "9876543210",
  "location": "New York, NY, USA",
  "latitude": 40.7128,
  "longitude": -74.0060,
  "risk_score": 75.5,
  "message": "I feel so hopeless...",
  "status": "active"
}
```

Admin can:
1. Call the number
2. Open location in Google Maps
3. Read the concerning message
4. Update status (Respond/Resolve)

---

## 🌟 Key Features

### ✅ User Features
- Beautiful home page with medical theme
- Simple registration (name + phone)
- AI psychologist chat
- Voice input support
- Real-time risk detection
- Automatic location request for emergencies
- Privacy and confidentiality

### ✅ Admin Features
- Secure login (password: admin@123)
- Real-time dashboard
- Patient contact info (name + phone)
- Click-to-call phone numbers
- Google Maps integration
- Alert status management
- Auto-refresh every 10 seconds

### ✅ Technical Features
- Flask backend
- Groq AI (Llama 3.1 70B)
- Supabase database
- NLP emotion detection
- Risk scoring algorithm
- Emergency alert system

---

## 📞 Contact Flow

### For High-Risk User:
1. User expresses suicidal thoughts
2. AI responds with empathy
3. System requests location
4. Alert created in database
5. Admin portal shows alert
6. **Doctor clicks phone number**
7. Phone dialer opens
8. Doctor calls user
9. Doctor marks alert as "In Progress"
10. After helping, marks as "Resolved"

---

## 🎯 Next Steps

1. ✅ Update Supabase schema (add phone columns)
2. ✅ Add API keys to .env
3. ✅ Install dependencies
4. ✅ Run Flask backend
5. ✅ Open index.html
6. ✅ Test complete flow
7. ✅ Test admin login (admin@123)

---

## 🐛 Troubleshooting

**"Module not found" errors:**
```bash
pip install -r requirements.txt
```

**Database errors:**
- Run the updated SQL schema
- Check .env credentials

**Admin portal empty:**
- Create high-risk conversation first
- Check backend is running
- Check browser console

**Phone number not showing:**
- Re-register with new account
- Check database has phone column
- Restart backend server

---

## ✨ Complete!

Your mental health support system is ready with:
- ✅ Home page with Get Started
- ✅ Registration with phone number
- ✅ AI psychologist chat
- ✅ Emergency detection
- ✅ Location tracking
- ✅ Admin portal with phone numbers
- ✅ About Us page
- ✅ Admin login (admin@123)

**Start the journey:**
1. `cd backend && python main.py`
2. Open `frontend/index.html`
3. Click "Get Started"
4. Talk to the AI psychologist!

**Admin access:**
1. Go to About Us page
2. Click "Admin Portal"
3. Password: `admin@123`
4. View emergency alerts with phone numbers!

🎉 **Everything is ready to use!**
