# 📖 How to Run: Iris Attendance System

This guide covers the complete setup process, including the specific **Connectivity Fixes** required for mobile-to-laptop communication.

---

## 🛠️ Phase 1: Backend Setup

1. **Install Python 3.10+**
2. **Setup Virtual Environment** (Recommended):
   ```bash
   cd backend
   python -m venv venv
   source venv/Scripts/activate  # Windows
   ```
3. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```
4. **Initialize Database**:
   *This script adds the necessary iris-feature columns to your student table.*
   ```bash
   python migrate_db.py
   ```
5. **Start the Server**:
   **CRITICAL**: You must use Port **8001** and Host **0.0.0.0** for mobile visibility.
   ```bash
   uvicorn main:app --reload --host 0.0.0.0 --port 8001
   ```

---

## 📱 Phase 2: Flutter App Setup

1. **Prerequisites**: Install Flutter SDK and set up your mobile device for debugging.
2. **Network Config**:
   - Ensure your **Phone** and **Laptop** are on the **SAME Wi-Fi**.
   - Check your laptop's IP address (Run `ipconfig`).
   - The app is currently set to `192.168.157.61:8001`. If your IP changes, update it in:
     `lib/core/services/api_service.dart`
3. **Run the App**:
   ```bash
   cd flutter_app
   flutter pub get
   flutter run
   ```

---

## 🔧 Phase 3: Solving Connectivity Errors (The "Ghost Fix")

If your phone says "Connection Refused" but the laptop is running:

### 1. Clear Ghost Processes
Sometimes a "Ghost" Uvicorn process hangs in the background on port 8000/8001.
```bash
# In the backend folder, run my custom cleanup script:
python kill_ghosts.py
```

### 2. Windows Firewall
Windows may block the phone from talking to the laptop. Run this in **PowerShell (Admin)**:
```powershell
New-NetFirewallRule -DisplayName "Iris-Backend-8001" -Direction Inbound -LocalPort 8001 -Protocol TCP -Action Allow
```

### 3. Verification Check
Open your **Mobile Browser** and visit:
`http:// <YOUR_LAPTOP_IP> :8001/`
If you see `{"message": "Iris Attendance System", ...}`, your connection is perfect!

---

## 🔑 Default Credentials
- **Admin**: `admin` / `admin123`
- **Staff**: Created through the Admin portal.

## 👁️ Scanning Best Practices
1. Hold the camera about **10-15cm** from the eye.
2. Ensure **good lighting** on the face.
3. Align the eye within the **guides** on the scan screen.
4. If a "Fake Eye" is detected, ensure there are no bright reflections directly on the pupil.

---

**🎉 You're all set!**
For system details, see the main [README.md](./README.md).
