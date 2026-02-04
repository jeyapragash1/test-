# 🏥 LIS Patient Testing Protocol & Recruitment Guide

## **1. PATIENT RECRUITMENT & APPROVAL PROCESS**

### **Phase 1: Finding LIS Patients (Week 1-2)**

Your group member (recruitment officer) should:

#### **Step 1a: Identify Healthcare Facilities**
- **Neurology departments** (hospitals)
- **ICU/Critical Care units** (ventilator-dependent patients)
- **Rehabilitation centers** (post-stroke recovery)
- **Home care services** (homebound patients)
- **ALS/SMA support organizations** (patient networks)

#### **Step 1b: Contact Healthcare Providers**
```
Sample Email Template:

Subject: Research Collaboration - Eye-Gaze Communication System for LIS Patients

Dear [Doctor/Hospital Name],

We are conducting research on an eye-tracking communication system 
for Locked-in Syndrome patients. We are seeking 3-5 patients who:

- Have confirmed LIS diagnosis or severely limited motor function
- Can move their eyes voluntarily
- Have normal cognitive function
- Are interested in testing assistive communication technology

Would your facility be interested in participating? 
We follow strict ethical protocols and have university approval.

Regards,
[Research Team]
```

#### **Step 1c: Patient & Family Consent**
- **Medical team** introduces project to patient/family
- **Patient/guardian** reads informed consent form
- **Discuss benefits & risks** openly
- **Obtain signed consent** (patient signature or family + doctor co-sign)
- **Document consent** in patient file

**Consent Form Should Include:**
```
✓ Study purpose & procedure
✓ Time commitment (3-5 meetings, 30-45 min each)
✓ Data collection (images, gaze data, recordings)
✓ Privacy & anonymization
✓ Right to withdraw anytime
✓ Emergency SOS protocol
✓ Contact for questions
✓ Patient/guardian signature & date
```

---

## **2. MEETING SCHEDULE: 3-5 Visits Over 4-6 Weeks**

### **MEETING 1: Initial Assessment & Baseline Setup (Week 1)**
**Duration:** 45-60 minutes  
**Location:** Patient's room or clinical setting  
**Who Should Attend:** Patient, family/caregiver, medical staff, YOU

#### **Tasks:**
- [ ] Medical assessment (eye movement capability, fatigue level, medications)
- [ ] Establish baseline communication method (letter board, eye-pointing, existing AAC)
- [ ] Test lighting conditions in patient's environment
- [ ] Calibrate eye-tracking system (4-point calibration)
- [ ] Collect **5-10 minutes baseline gaze data** (raw images + tracking)
- [ ] Record patient preferences (communication speed, button size feedback)
- [ ] Set up emergency SOS protocol (caregiver knows panic button)
- [ ] Schedule next meeting

#### **Data Collected:**
```
Baseline/Meeting1_PatientID/
├── calibration_frames/          # Calibration point images
├── baseline_tracking.csv         # Gaze coordinates (5-10 min)
├── baseline_images/              # 1000-3000 eye images
├── patient_profile.json          # Age, condition, capabilities
└── consent_signed.pdf            # Signed consent form
```

#### **Success Criteria:**
- ✅ System runs without crashes
- ✅ Patient comfortable with eye-tracking camera
- ✅ Calibration error < 50 pixels
- ✅ Patient can see all buttons clearly

---

### **MEETING 2: Extended Testing & Data Collection (Week 2)**
**Duration:** 30-45 minutes  
**Gap from Meeting 1:** 3-5 days (avoid fatigue)

#### **Tasks:**
- [ ] Recalibrate system (patient may have moved, different lighting)
- [ ] **Main task: Collect 15-20 minutes of gaze data**
  - Test keyboard screen (type messages)
  - Test QuickTalk buttons (send YES/NO/HELP)
  - Test TTS (speak typed messages)
- [ ] Record natural head position changes
- [ ] Monitor eye fatigue (ask patient if tired)
- [ ] Test different lighting conditions if possible
- [ ] Collect **10,000-15,000 images** for model training
- [ ] Get feedback on UI (buttons too big/small? text clear?)

#### **Data Collected:**
```
Meeting2_PatientID/
├── recalibration_data.csv       # New calibration points
├── keyboard_interaction.csv      # Button clicks on keyboard
├── quick_talk_interaction.csv    # Quick command usage
├── tts_usage.csv                # Text-to-speech activations
├── gaze_tracking_extended.csv   # 15-20 min tracking data
├── extended_images/              # 10000-15000 eye images
├── feedback_notes.txt            # UI feedback from patient
└── eye_fatigue_log.txt          # Fatigue observations
```

#### **Success Criteria:**
- ✅ System handles 15+ minute sessions
- ✅ Accuracy consistent throughout session
- ✅ No excessive gaze drift
- ✅ Patient feedback collected

---

### **MEETING 3: Model Training Phase (1 week later)**
**Duration:** 15-30 minutes  
**Gap from Meeting 2:** 5-7 days

**Note:** You train the model during this week, don't test patient this week

#### **What YOU Do (Not the patient meeting):**
1. **Collect all data from Meetings 1-2**
2. **Train personalized gaze model** using their eye images
   - Use deep learning (CNN) on collected eye images
   - Learn their specific iris patterns, pupil size variations
   - Adapt to their head position quirks
3. **Test model offline** with their data
4. **Validate accuracy**
   - Expected improvement: 85% → 92-95%
5. **Prepare optimized system** for Meeting 3

#### **Meeting 3 Task:**
- [ ] **Validate improved accuracy** with patient
- [ ] Test if gaze points match screen positions better
- [ ] Get feedback: "Is it more accurate now?"
- [ ] Test newly trained model for 10-15 minutes
- [ ] Collect additional 5000-8000 images for future fine-tuning
- [ ] Troubleshoot any issues identified in training

#### **Data Collected:**
```
Meeting3_PatientID/
├── validation_tracking.csv       # Testing new trained model
├── validation_images/            # 5000-8000 new images
├── model_performance.json        # Accuracy metrics
│   {
│     "accuracy_before": "0.87",
│     "accuracy_after": "0.94",
│     "calibration_error_px": 15
│   }
└── validation_feedback.txt       # Patient feedback on improvements
```

#### **Success Criteria:**
- ✅ Model accuracy improved 5-10%
- ✅ Patient notices better tracking
- ✅ No calibration drift issues

---

### **MEETING 4: Feature Training & System Mastery (Week 4)**
**Duration:** 40-50 minutes  
**Gap from Meeting 3:** 5-7 days

#### **Tasks:**
- [ ] **Teach system features** step-by-step:
  1. **Keyboard** - Type full sentences
  2. **Word prediction** - Show how suggestions work
  3. **Quick Talk** - Demonstrate YES/NO/HELP/PAIN/SOS
  4. **Text-to-Speech** - Show voice feedback
  5. **Settings** - Adjust sensitivity if needed
  6. **Low-light mode** - If needed for their environment
- [ ] Let patient PRACTICE each feature (10-15 min hands-on)
- [ ] Collect **feedback on usability** for each feature
- [ ] Identify pain points (buttons too small? too fast? too slow?)
- [ ] Test with real messages patient wants to send
- [ ] Collect 10,000+ images with natural interaction

#### **Data Collected:**
```
Meeting4_PatientID/
├── feature_interaction_logs.csv  # Which features used, duration
├── usability_feedback.json
│   {
│     "keyboard": "Good, but buttons could be bigger",
│     "quick_talk": "Perfect size",
│     "tts": "Speed is good (100 WPM)",
│     "word_prediction": "Helpful",
│     "settings": "Didn't need to change"
│   }
├── real_message_samples/         # Actual messages sent
├── feature_interaction_images/   # 10000+ images
└── interaction_times.csv         # Response times for each button
```

#### **Success Criteria:**
- ✅ Patient understands all main features
- ✅ Can independently type & send message
- ✅ Can use Quick Talk buttons
- ✅ Can activate TTS
- ✅ Feedback collected for improvements

---

### **MEETING 5: Longitudinal Testing & Optimization (Week 5-6) [OPTIONAL]**
**Duration:** 30-45 minutes  
**Gap from Meeting 4:** 7 days

#### **Tasks:**
- [ ] **Long-term validation** - Test after one week rest
- [ ] Check if calibration drifts over time
- [ ] Re-test accuracy (should be sustained or improved)
- [ ] Collect 10,000+ more images for further model refinement
- [ ] **Gather final feedback** - What worked? What needs improvement?
- [ ] Ask: "Would you use this system daily?"
- [ ] Discuss next steps (ongoing usage? deployment? publication?)

#### **Data Collected:**
```
Meeting5_PatientID/
├── long_term_tracking.csv       # After one-week gap
├── accuracy_validation.json     # Sustained performance check
├── long_term_images/            # 10000+ images
├── final_feedback.txt           # Overall system assessment
└── deployment_readiness.txt     # Ready for daily use? Yes/No
```

#### **Success Criteria:**
- ✅ Performance maintained over time
- ✅ No significant accuracy degradation
- ✅ Patient satisfied with system
- ✅ System ready for possible deployment

---

## **3. COMPLETE TIMELINE OVERVIEW**

```
WEEK 1:   Meeting 1 (Initial Setup)
          ↓ 3-5 days rest
WEEK 2:   Meeting 2 (Extended Testing) → Collect training data
          ↓ 5-7 days (You train model)
WEEK 3:   Meeting 3 (Validation) → Test new trained model
          ↓ 5-7 days (Optional: Fine-tune model)
WEEK 4:   Meeting 4 (Feature Training) → Full system mastery
          ↓ 7 days (Optional: Analyze all data)
WEEK 5-6: Meeting 5 (Longitudinal) [OPTIONAL] → Long-term validation

TOTAL TIME: 4-6 weeks per patient
TOTAL MEETINGS: 4-5 times
TOTAL DATA PER PATIENT: 40,000-60,000 images + 60+ minutes tracking data
```

---

## **4. WHAT HAPPENS BETWEEN MEETINGS**

### **YOUR TEAM'S WORK (While patient rests):**

```
Meeting 2 ends → You have patient's data
                ↓
        WEEK 1: DATA PROCESSING
        - Organize all images & tracking CSVs
        - Label eye images (iris, pupil, eyelid)
        - Extract gaze features
        - Check for corrupted files
                ↓
        WEEK 2: MODEL TRAINING
        - Train CNN on patient's eye images
        - Learn personalized iris patterns
        - Create patient-specific gaze model
        - Validate on held-out test data
                ↓
        WEEK 3: OPTIMIZATION
        - Fine-tune hyperparameters
        - Improve accuracy
        - Prepare improved system
        - Test offline
                ↓
        Meeting 3: TEST & VALIDATE
```

---

## **5. WHEN TO COLLECT EACH DATA TYPE**

| Data Type | Meeting 1 | Meeting 2 | Meeting 3 | Meeting 4 | Meeting 5 |
|---|---|---|---|---|---|
| **Baseline calibration** | ✅ | ✅ (recal) | ✅ | - | - |
| **Gaze tracking** | ✅ (5m) | ✅ (15m) | ✅ (10m) | ✅ (20m) | ✅ (15m) |
| **Eye images** | ✅ (1000) | ✅ (10000) | ✅ (5000) | ✅ (10000) | ✅ (10000) |
| **Keyboard interaction** | - | ✅ | ✅ | ✅ | ✅ |
| **Button clicks** | - | ✅ | ✅ | ✅ | ✅ |
| **TTS usage** | - | ✅ | ✅ | ✅ | ✅ |
| **Blink events** | ✅ | ✅ | ✅ | ✅ | ✅ |
| **Feedback survey** | ✅ | ✅ | ✅ | ✅ | ✅ |

---

## **6. PATIENT RECRUITMENT CHECKLIST**

### **Before First Meeting:**
- [ ] Obtain ethics approval (✅ You have this)
- [ ] Identify healthcare facility/patient source
- [ ] Contact medical team at facility
- [ ] Share informed consent document
- [ ] Get patient & family consent (signed)
- [ ] Coordinate scheduling with medical staff
- [ ] Set up secure data storage location
- [ ] Create anonymous patient ID (e.g., "LIS_P001")
- [ ] Brief all team members on protocol
- [ ] Prepare emergency procedures

### **Before Each Meeting:**
- [ ] Confirm appointment with patient/facility (24h prior)
- [ ] Check system works (test on healthy person first)
- [ ] Backup previous data
- [ ] Prepare consent & feedback forms
- [ ] Set up camera & lighting
- [ ] Have emergency contact numbers ready
- [ ] Brief medical staff on monitoring needs

### **After Each Meeting:**
- [ ] Securely transfer data to encrypted storage
- [ ] Anonymize patient identifiable info
- [ ] Document any issues/concerns
- [ ] Send thank you note to patient/family
- [ ] Schedule next meeting
- [ ] Start data processing

---

## **7. SAFETY PROTOCOLS**

### **Emergency Procedures:**
- ✅ **SOS Button visible** - Patient can call for help
- ✅ **Medical staff present** - Especially for fatigued patients
- ✅ **Maximum session time** - Don't exceed 45 minutes (eye fatigue)
- ✅ **Break times** - Take 5-10 min breaks between tasks
- ✅ **Eye rest** - Encourage patient to look away frequently
- ✅ **Stop anytime** - Patient can end session immediately
- ✅ **Emergency contacts** - Have nurse/doctor immediately available

---

## **8. DATA PRIVACY & STORAGE**

### **During Data Collection:**
```
Patient Name → Assigned ID (LIS_P001)
All files named with ID only
No identifiable info in images/data files
Secure location only (encrypted drive)
```

### **File Naming Convention:**
```
GOOD: 
Meeting2_LIS_P001_gaze_tracking.csv
Meeting2_LIS_P001_image_00001.jpg

BAD (identifiable):
John_Smith_eye_tracking.csv
Ali_Mohammad_patient_image.jpg
```

---

## **9. SUCCESS METRICS (What to Measure)**

### **System Performance:**
- ✅ Calibration accuracy (< 50 pixels error)
- ✅ Gaze tracking consistency (< 10% variance)
- ✅ Button click success rate (> 90%)
- ✅ Model accuracy improvement (85% → 95%+)

### **Patient Experience:**
- ✅ Usability rating (1-10 scale)
- ✅ Feature satisfaction (keyboard, TTS, quick-talk)
- ✅ Eye fatigue level (low/medium/high)
- ✅ Willingness to use daily (yes/maybe/no)

### **Data Quality:**
- ✅ Total images collected (> 40,000)
- ✅ Tracking data duration (> 60 minutes)
- ✅ Corrupted files (< 5%)
- ✅ Complete sessions (no incomplete meetings)

---

## **10. SAMPLE COMMUNICATION EMAIL TO PATIENT**

```
Subject: Thank You for Participating - Eye-Gaze Research Study

Dear [Patient Name / Family],

Thank you for your participation in our eye-gaze communication 
research study! Your involvement is invaluable.

MEETING SCHEDULE:
- Meeting 1: [Date/Time] ✅ Complete
- Meeting 2: [Date/Time] ✅ Complete
- Break for analysis (we train the model)
- Meeting 3: [Date/Time] 📅 Upcoming
- Meeting 4: [Date/Time] 📅 Scheduled
- Meeting 5: [Date/Time] 📅 Optional

At each meeting, we will:
1. Test eye-tracking accuracy
2. Collect images for AI training
3. Gather your feedback
4. Improve the system based on your input

We know this requires your time and energy. We truly appreciate your 
participation and patience as we develop better communication tools 
for people with LIS.

If you have any questions or need to reschedule, please contact:
[Team Member Name]: [Phone/Email]

Warm regards,
[Research Team]
```

---

## **FINAL ANSWER TO YOUR QUESTION:**

### **How many times to meet patients? 4-5 MEETINGS**

**Optimal Schedule:**
1. **Meeting 1** (Week 1): Initial setup & baseline data
2. **Meeting 2** (Week 2): Main data collection for model training
3. **[Training happens - you work, patient rests]**
4. **Meeting 3** (Week 3): Validate improved model
5. **Meeting 4** (Week 4): Teach features & gather usability feedback
6. **Meeting 5** (Week 5-6): OPTIONAL - Long-term validation

**Why this many?**
- ✅ Meeting 1: Establish baseline
- ✅ Meeting 2: Collect training data
- ✅ Meeting 3: Validate improvements
- ✅ Meeting 4: Test usability & get feedback
- ✅ Meeting 5: Prove long-term reliability

**If time-constrained:** Minimum 3 meetings
- Meeting 1: Setup
- Meeting 2: Main data collection
- Meeting 4: Features & feedback (skip Meeting 3 validation)

**Total commitment:** 4-6 weeks per patient, ~180 minutes per patient
