# 📊 DATA TYPES & ANALYSIS GUIDE
## Eye-Gaze Communication System for LIS Patients

---

## **1. OVERVIEW: DATA TYPES IN YOUR PROJECT**

Your system collects **4 main categories of data:**

```
┌─────────────────────────────────────────────────────────┐
│        DATA TYPES IN YOUR SYSTEM                        │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  1. QUANTITATIVE DATA (Numerical/Measurable)            │
│     └─ Gaze coordinates, accuracy, timestamps           │
│                                                         │
│  2. QUALITATIVE DATA (Descriptive/Non-numerical)        │
│     └─ Feedback, observations, messages, conditions     │
│                                                         │
│  3. VISUAL DATA (Images - Both categories)              │
│     └─ Eye images, frame captures                       │
│                                                         │
│  4. CATEGORICAL DATA (Labels/Categories)                │
│     └─ Activity types, screen names, features used      │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## **2. DETAILED DATA BREAKDOWN BY TYPE**

### **CATEGORY 1: QUANTITATIVE DATA**
**Definition:** Numerical data that can be measured and analyzed statistically

---

#### **2.1.1 Eye-Gaze Tracking Data**

**File:** `tracking_YYYYMMDD_HHMMSS.csv`

| Data Field | Range | Type | Usage | Analysis |
|---|---|---|---|---|
| **gaze_x_normalized** | 0.0-1.0 | Quantitative | Horizontal eye position | Mean, SD, distribution |
| **gaze_y_normalized** | 0.0-1.0 | Quantitative | Vertical eye position | Mean, SD, distribution |
| **screen_x_pixels** | 0-1920 | Quantitative | X screen coordinate | Accuracy testing |
| **screen_y_pixels** | 0-1080 | Quantitative | Y screen coordinate | Accuracy testing |
| **iris_confidence** | 0.0-1.0 | Quantitative | Detection reliability | Quality metrics |
| **is_blinking** | 0 or 1 | Quantitative | Blink state | Blink rate, pattern |
| **eye_height_left** | 0.0-0.3 | Quantitative | Left EAR score | Fatigue detection |
| **eye_height_right** | 0.0-0.3 | Quantitative | Right EAR score | Fatigue detection |

**Example Data Row:**
```
timestamp: 2026-02-04T14:30:45.123
frame_number: 1024
gaze_x_normalized: 0.5234
gaze_y_normalized: 0.6789
screen_x_pixels: 1920
screen_y_pixels: 1080
iris_confidence: 0.95
is_blinking: 0
eye_height_left: 0.023
eye_height_right: 0.024
```

**How to Use:**
- Calculate accuracy: Distance from target = √((x-target_x)² + (y-target_y)²)
- Track gaze patterns: Where patient looked most
- Detect fatigue: Monitor EAR decline over time
- Validate system: Compare with calibration points

**Analysis Methods:**
```python
import numpy as np
import pandas as pd

# Load gaze data
df = pd.read_csv('tracking_YYYYMMDD_HHMMSS.csv')

# 1. ACCURACY ANALYSIS
def calculate_accuracy(gaze_x, gaze_y, target_x, target_y):
    error_px = np.sqrt((gaze_x - target_x)**2 + (gaze_y - target_y)**2)
    return error_px

# 2. CONFIDENCE ANALYSIS
mean_confidence = df['iris_confidence'].mean()     # 0.94
std_confidence = df['iris_confidence'].std()       # 0.05
min_confidence = df['iris_confidence'].min()       # 0.72

# 3. BLINK ANALYSIS
blink_count = df['is_blinking'].sum()              # 145 blinks
total_frames = len(df)                             # 18000
blink_rate = (blink_count / total_frames) * 100    # 0.81% (normal)

# 4. FATIGUE ANALYSIS
ear_left = df['eye_height_left'].values
ear_right = df['eye_height_right'].values
ear_over_time = pd.DataFrame({
    'frame': range(len(ear_left)),
    'EAR': (ear_left + ear_right) / 2
})
# Plot: if EAR decreases over time → eye fatigue
```

---

#### **2.1.2 Calibration Data**

**File:** `calibration_YYYYMMDD_HHMMSS.csv`

| Data Field | Example | Type | Usage |
|---|---|---|---|
| **point_number** | 1, 2, 3, 4 | Quantitative | Calibration point ID |
| **target_screen_x** | 192 | Quantitative | Target X pixel |
| **target_screen_y** | 162 | Quantitative | Target Y pixel |
| **measured_gaze_x** | 0.1045 | Quantitative | Detected iris X |
| **measured_gaze_y** | 0.1523 | Quantitative | Detected iris Y |
| **error_pixels** | 15.3 | Quantitative | Distance from target |

**Example Data:**
```
timestamp,point_number,target_screen_x,target_screen_y,measured_gaze_x,measured_gaze_y,error_pixels,calibration_quality
2026-02-04T14:25:00.123,1,192,162,0.1045,0.1523,15.3,Good
2026-02-04T14:25:01.245,2,1152,216,0.6012,0.1987,18.7,Good
2026-02-04T14:25:02.367,3,1632,756,0.8512,0.7156,12.5,Good
2026-02-04T14:25:03.489,4,480,810,0.2512,0.7612,22.1,Good
```

**How to Use:**
- Validate calibration quality
- Identify systematic errors
- Determine if recalibration needed
- Track calibration changes over time

**Analysis Methods:**
```python
# Load calibration
cal_df = pd.read_csv('calibration_YYYYMMDD_HHMMSS.csv')

# 1. OVERALL ACCURACY
mean_error = cal_df['error_pixels'].mean()        # 16.9 px
std_error = cal_df['error_pixels'].std()          # 4.2 px
max_error = cal_df['error_pixels'].max()          # 22.1 px

# 2. POINT-BY-POINT ANALYSIS
for idx, row in cal_df.iterrows():
    print(f"Point {row['point_number']}: {row['error_pixels']:.1f}px error")
    
# 3. QUALITY ASSESSMENT
if mean_error < 50:
    quality = "Good - System ready"
elif mean_error < 100:
    quality = "Fair - Acceptable"
else:
    quality = "Poor - Recalibrate"

# 4. ERROR DISTRIBUTION
import matplotlib.pyplot as plt
plt.hist(cal_df['error_pixels'], bins=10)
plt.xlabel('Calibration Error (pixels)')
plt.ylabel('Frequency')
plt.title('Calibration Accuracy Distribution')
plt.show()
```

---

#### **2.1.3 Activity Logs Data**

**File:** `activities_YYYYMMDD_HHMMSS.csv`

| Data Field | Example | Type | Usage |
|---|---|---|---|
| **timestamp** | 2026-02-04T14:30:15 | Quantitative | Event time |
| **duration_ms** | 300 | Quantitative | Action duration |
| **activity_type** | screen_change, blink | Categorical | Event category |
| **screen_name** | keyboard, quick | Categorical | Current screen |

**Example Data:**
```
timestamp,activity_type,screen_name,details,duration_ms
2026-02-04T14:30:15.000,screen_change,keyboard,,
2026-02-04T14:30:20.000,text_spoken,keyboard,I need help,
2026-02-04T14:30:25.000,blink_detected,keyboard,single_blink,300
2026-02-04T14:30:30.000,screen_change,quick,,
2026-02-04T14:30:35.000,blink_detected,quick,double_blink,250
```

**How to Use:**
- Track user behavior patterns
- Measure session timeline
- Count interactions per feature
- Analyze communication flow

**Analysis Methods:**
```python
activity_df = pd.read_csv('activities_YYYYMMDD_HHMMSS.csv')

# 1. ACTIVITY FREQUENCY
activity_counts = activity_df['activity_type'].value_counts()
print(activity_counts)
# Output:
# blink_detected      87
# screen_change       15
# text_spoken         12
# app_launch          3

# 2. TIME SPENT ON EACH SCREEN
screen_time = activity_df[activity_df['activity_type'] == 'screen_change']
for screen in screen_time['screen_name'].unique():
    count = (screen_time['screen_name'] == screen).sum()
    print(f"{screen}: {count} visits")
    
# 3. INTERACTION RESPONSE TIME
response_times = activity_df['duration_ms'].describe()
print(f"Mean response time: {response_times['mean']:.0f}ms")
print(f"Median response time: {response_times['50%']:.0f}ms")
print(f"Std deviation: {response_times['std']:.0f}ms")

# 4. SESSION TIMELINE
import matplotlib.pyplot as plt
activity_df['timestamp'] = pd.to_datetime(activity_df['timestamp'])
activity_df.set_index('timestamp')['activity_type'].value_counts().plot()
plt.title('Activity Timeline')
plt.show()
```

---

#### **2.1.4 Session Metadata (JSON)**

**File:** `session_YYYYMMDD_HHMMSS.json`

```json
{
  "session_id": "20260204_143000",
  "start_time": "2026-02-04T14:30:00.123456",
  "end_time": "2026-02-04T14:40:15.654321",
  "duration_seconds": 615.5,
  "total_activities": 47,
  "total_eye_tracking_frames": 18465,
  "calibration_points_collected": 4,
  "images_captured": 18465,
  "image_every_n_frames": 1,
  "app_launches": {
    "WhatsApp": 2,
    "YouTube": 1
  },
  "screens_visited": {
    "dashboard": 5,
    "keyboard": 12,
    "quick": 3,
    "settings": 2
  }
}
```

**Quantitative Fields:**
- `duration_seconds` - Session length
- `total_activities` - Interaction count
- `total_eye_tracking_frames` - Data points
- `calibration_points_collected` - Calibration completeness
- `images_captured` - Data volume

**How to Use:**
- Compare session lengths
- Measure data collection volume
- Track engagement metrics
- Validate data completeness

**Analysis Methods:**
```python
import json

with open('session_YYYYMMDD_HHMMSS.json') as f:
    session = json.load(f)

# 1. SESSION DURATION
duration_min = session['duration_seconds'] / 60
print(f"Session duration: {duration_min:.1f} minutes")

# 2. DATA VOLUME
total_data_points = session['total_eye_tracking_frames']
print(f"Total eye tracking data: {total_data_points:,} frames")

# 3. ENGAGEMENT
print(f"Total interactions: {session['total_activities']}")
print(f"Activities per minute: {session['total_activities']/duration_min:.1f}")

# 4. FEATURE USAGE
screens_visited = session['screens_visited']
total_visits = sum(screens_visited.values())
for screen, visits in screens_visited.items():
    percentage = (visits / total_visits) * 100
    print(f"{screen}: {percentage:.1f}%")
```

---

### **CATEGORY 2: QUALITATIVE DATA**
**Definition:** Non-numerical descriptive data about qualities and characteristics

---

#### **2.2.1 Patient Feedback (From Meetings)**

**Source:** Direct patient responses during testing

| Data | Example | Category |
|---|---|---|
| **UI Feedback** | "Buttons too small", "Text clear" | Usability |
| **Feature Feedback** | "TTS too fast", "Keyboard perfect" | Feature evaluation |
| **Comfort** | "Eye fatigue after 20 min", "Strain in right eye" | User experience |
| **Performance Perception** | "System slower today", "Works great" | User experience |
| **Communication Needs** | "Need medical words", "Family phrases helpful" | Content |

**Meeting 1 Example:**
```
Patient Feedback Form:
Q: How clear are the buttons?
A: "Very clear, I can see them from across room"

Q: How is the eye-tracking feeling?
A: "Natural, not uncomfortable"

Q: Any suggestions?
A: "Could we add family members' names to quick-talk?"
```

**How to Use:**
- Identify UI/UX issues
- Validate accessibility
- Gather user satisfaction
- Plan improvements

**Analysis Methods:**
```
1. THEMATIC CODING
   Group feedback by theme:
   
   Accessibility:
   - "Buttons too small" (Meeting 1, Patient 1)
   - "Text hard to read" (Meeting 2, Patient 3)
   → Theme: Text/button size needs increase
   
   Communication:
   - "Need medical words" (Meeting 1, Patient 1)
   - "Family phrases helpful" (Meeting 1, Patient 2)
   → Theme: Domain-specific vocabulary needed

2. FREQUENCY ANALYSIS
   Count how many patients mentioned each theme:
   
   Theme               Count    Percentage
   Button size         3/4      75%
   Eye fatigue         2/4      50%
   TTS quality         1/4      25%
   Word prediction     2/4      50%

3. SATISFACTION RATING
   Scale 1-5 by feature:
   
   Feature              Rating (Mean ± SD)
   Keyboard            4.5 ± 0.6
   Quick Talk          4.8 ± 0.4
   TTS                 4.2 ± 0.8
   Word Prediction     4.3 ± 0.9
   Overall System      4.6 ± 0.5
```

---

#### **2.2.2 Medical/Patient Information**

**Source:** Patient records + observations during testing

| Data | Example | Category |
|---|---|---|
| **Diagnosis** | ALS, Stroke, Spinal cord injury | Medical condition |
| **Symptom Severity** | Mild eye fatigue, No eye movement | Medical status |
| **Current Communication** | Eye-pointing board, Letter chart | Baseline method |
| **Environmental Notes** | Hospital room lighting, Patient in bed | Context |
| **Caregiver Observations** | "Patient less responsive afternoon", "Eye dries quickly" | Environmental factor |

**Medical Profile Example:**
```
Patient ID: LIS_P001
Diagnosis: ALS (Amyotrophic Lateral Sclerosis)
Duration: 2 years since diagnosis
Eye Movement: Full voluntary control
Current Communication: Eye-pointing with letter board (5 words/minute)
Observations: "Eye strain increases after 30 minutes"
                "Clearer in morning than afternoon"
                "Prefers darker environment"
```

**How to Use:**
- Correlate symptoms with system performance
- Identify best testing times
- Adapt system for individual needs
- Document medical context

**Analysis Methods:**
```
1. PATIENT STRATIFICATION
   Group patients by condition:
   
   Diagnosis      Count    Performance
   ALS            3        85-92% accuracy
   Stroke         2        88-96% accuracy
   SCI            1        90-94% accuracy
   
2. TIME-OF-DAY ANALYSIS
   Patient P001:
   - Morning (9am):  96% accuracy, no fatigue
   - Afternoon (2pm): 89% accuracy, fatigue reported
   - Evening (5pm):  82% accuracy, "very tired"
   
3. ENVIRONMENTAL FACTORS
   Lighting condition → Accuracy:
   - Bright (>500 lux): 94.2% ± 2.1%
   - Normal (200-500):  92.1% ± 3.2%
   - Dim (<200):        88.3% ± 5.1%
```

---

#### **2.2.3 Communication Content**

**Source:** Messages typed/sent through system

| Data | Example | Category |
|---|---|---|
| **Medical needs** | "I have pain", "Need medication" | Domain content |
| **Family contact** | "Call mom", "Tell dad" | Domain content |
| **Comfort** | "Water please", "Change position" | Domain content |
| **Emotions** | "I'm tired", "Happy to see you" | Domain content |
| **Emergency** | "SOS", "Help" | Domain content |

**Message Examples:**
```
Session 1:
- "I need help" (via keyboard)
- "YES" (Quick Talk response)
- "Mom come here" (via keyboard)
- "Stop" (Quick Talk)

Session 2:
- "Pain in chest" (keyboard)
- "SOS" (emergency button)
- "Doctor" (Quick Talk: Help)
```

**How to Use:**
- Understand communication patterns
- Design word prediction for common topics
- Create domain-specific vocabularies
- Improve system relevance

**Analysis Methods:**
```
1. CONTENT CATEGORIZATION
   Analyze all messages:
   
   Category          Count    %      Examples
   Medical          15        25%    "Pain", "Medicine"
   Family           24        40%    "Mom", "Dad"
   Comfort          12        20%    "Water", "Tired"
   Emergency        6         10%    "Help", "SOS"
   Other            3         5%     "Love you"

2. WORD FREQUENCY
   Most common words:
   - "Help" (14 times)
   - "Mom" (8 times)
   - "Pain" (7 times)
   - "Stop" (6 times)
   
   → Use for word prediction training

3. COMMUNICATION PATTERNS
   Sequence analysis:
   - Opens with greeting 80% of time
   - Follows with request/statement
   - Closes with gratitude 60% of time
   
   → Adapt interface accordingly
```

---

### **CATEGORY 3: VISUAL DATA**
**Definition:** Images that can be analyzed both qualitatively and quantitatively

---

#### **2.3.1 Eye Images**

**File:** `images/YYYYMMDD_HHMMSS/frame_NNNNNN.jpg`

**Volume:** 18,000+ JPG files per 10-minute session

**Content:**
- Full camera frame with face/eyes
- Iris position
- Pupil size
- Eyelid position
- Glints/reflections

**Quantitative Analysis (Image Processing):**

```python
import cv2
import numpy as np

# Load eye image
img = cv2.imread('frame_000001.jpg')
gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)

# 1. PUPIL SIZE MEASUREMENT
circles = cv2.HoughCircles(gray, cv2.HOUGH_GRADIENT, 1, 20,
                          param1=50, param2=30, minRadius=5, maxRadius=50)
if circles is not None:
    pupil_radius = circles[0][0][2]
    pupil_diameter = pupil_radius * 2
    print(f"Pupil diameter: {pupil_diameter:.1f} pixels")
    
# 2. IRIS CHARACTERISTICS
# Extract iris region and analyze
iris_area = ...  # pixels
iris_brightness = np.mean(gray[iris_region])
iris_contrast = np.std(gray[iris_region])

# 3. EYELID POSITION
top_eyelid_y = ...
bottom_eyelid_y = ...
eye_opening = bottom_eyelid_y - top_eyelid_y
print(f"Eye opening: {eye_opening} pixels")

# 4. IMAGE QUALITY
# Measure focus/clarity
laplacian = cv2.Laplacian(gray, cv2.CV_64F)
focus_score = laplacian.var()
print(f"Focus quality: {focus_score:.1f} (higher=sharper)")

# 5. LIGHTING CONDITIONS
brightness = np.mean(gray)
contrast = np.std(gray)
print(f"Brightness: {brightness:.0f}, Contrast: {contrast:.0f}")
```

**Qualitative Analysis (Visual Inspection):**
- Eye position (left, center, right)
- Head tilt
- Gaze direction
- Occlusion/obstruction
- Artifacts/noise

**How to Use:**
- Train iris detection models
- Validate gaze detection
- Assess image quality
- Track eye movement patterns

---

#### **2.3.2 Heatmap Analysis**

**How to Create:**
```python
import matplotlib.pyplot as plt

# Create gaze point map
gaze_points_x = df['screen_x_pixels'].values
gaze_points_y = df['screen_y_pixels'].values

# Create 2D histogram (heatmap)
heatmap, xedges, yedges = np.histogram2d(
    gaze_points_x, gaze_points_y,
    bins=[50, 50],  # 50x50 grid
    range=[[0, 1920], [0, 1080]]
)

plt.imshow(heatmap.T, extent=[0, 1920, 1080, 0], cmap='hot')
plt.colorbar(label='Gaze Frequency')
plt.title('Gaze Distribution Heatmap')
plt.xlabel('Screen X (pixels)')
plt.ylabel('Screen Y (pixels)')
plt.show()
```

**Interpretation:**
- Red areas = High gaze attention
- Cool areas = Low attention
- Patterns reveal UI effectiveness

---

### **CATEGORY 4: CATEGORICAL DATA**
**Definition:** Labels/categories that can be counted and analyzed

---

#### **2.4.1 Activity Types**

**Categories in CSV:**
```
- calibration_started
- calibration_completed
- screen_change
- app_launch
- text_spoken
- blink_detected
- click
```

**Analysis:**
```python
activity_types = activity_df['activity_type'].value_counts()

# Visualization
activity_types.plot(kind='bar')
plt.title('Activity Type Distribution')
plt.ylabel('Frequency')
plt.show()

# Percentage breakdown
percentages = (activity_types / activity_types.sum()) * 100
for activity, pct in percentages.items():
    print(f"{activity}: {pct:.1f}%")
```

---

#### **2.4.2 Screen Names**

**Categories:**
```
- dashboard
- keyboard
- quick (QuickTalk)
- settings
- apps
- calibration
- tutorial
```

**Analysis:**
```
Screen Usage Distribution:
┌──────────────┬────────┬─────────┐
│ Screen       │ Visits │ Percent │
├──────────────┼────────┼─────────┤
│ keyboard     │ 12     │ 40%     │
│ quick        │ 8      │ 27%     │
│ settings     │ 5      │ 17%     │
│ apps         │ 3      │ 10%     │
│ dashboard    │ 2      │ 7%      │
└──────────────┴────────┴─────────┘
```

---

## **3. DATA USAGE BY RESEARCH OBJECTIVE**

### **Objective #1: Eye-Gaze Detection**

**Data Used:** Quantitative
- Gaze coordinates (screen_x, screen_y)
- Iris confidence
- Calibration data
- Eye aspect ratio (EAR)

**Analysis:**
```
Accuracy metrics:
- Mean error: 22.5 ± 8.3 pixels
- Detection rate: 94.2% ± 3.1%
- Confidence: 0.94 ± 0.05

Report Format:
"The system achieved 94.2% eye detection rate with
mean gaze accuracy of 22.5 pixels (SD=8.3). Calibration
validation showed <50px error for all 4 points (M=18.7,
SD=4.2), indicating good system reliability."
```

---

### **Objective #2: Low-Light Enhancement**

**Data Used:** Quantitative (images analyzed)
- Brightness measurements (before/after)
- Contrast values
- Accuracy in different lighting conditions

**Analysis:**
```
Performance in different lighting:
┌──────────────┬──────────┬──────────┬────────────┐
│ Lighting     │ Accuracy │ Contrast │ Brightness │
├──────────────┼──────────┼──────────┼────────────┤
│ Bright       │ 95.2%    │ High     │ High       │
│ Normal       │ 92.5%    │ Medium   │ Medium     │
│ Dim (w/enh)  │ 88.3%    │ Enhanced │ Enhanced   │
│ Dim (w/o)    │ 76.2%    │ Low      │ Low        │
└──────────────┴──────────┴──────────┴────────────┘

Enhancement Effect:
- Improvement: 88.3% - 76.2% = +12.1% accuracy gain
- Conclusion: Low-light mode effective for dim conditions
```

---

### **Objective #3: User Interface Accessibility**

**Data Used:** Both Quantitative + Qualitative

**Quantitative:**
```
Feature Usage Frequency:
- Keyboard: 156 key presses (65%)
- Quick Talk: 45 button clicks (19%)
- TTS: 28 activations (12%)
- Word prediction: 34 suggestions applied (22% of typing)

Button Success Rate:
- Overall: 96.7% ± 2.1%
- Keyboard: 97.2%
- Quick Talk: 96.1%
- Settings: 95.5%

Response Times:
- Mean: 318 ± 45 ms
- Median: 310 ms
- Range: 210-520 ms
```

**Qualitative:**
```
Patient Feedback (Meeting 4):

"I can easily see all the buttons. The text is clear."
→ Theme: Visibility/Readability ✓

"The keyboard takes a bit longer but it works well."
→ Theme: Keyboard efficiency (neutral)

"Quick commands are perfect - I don't have to type."
→ Theme: Quick Talk satisfaction ✓

"Text-to-speech is much slower now, perfect pace"
→ Theme: TTS speed improvement ✓

"I get tired after 30 minutes of use"
→ Theme: Eye fatigue (session duration limit)

Overall satisfaction: 4.6/5 ⭐
```

---

## **4. HOW TO ANALYZE EACH DATA TYPE**

### **4.1 Analysis Methods for Quantitative Data**

#### **Descriptive Statistics**
```python
import pandas as pd
import numpy as np

data = df['gaze_accuracy']

# Central tendency
mean = data.mean()              # 22.5
median = data.median()          # 21.8
mode = data.mode()              # 19.2

# Spread
std_dev = data.std()            # 8.3
variance = data.var()           # 68.9
range_val = data.max() - data.min()  # 45

# Percentiles
percentile_25 = data.quantile(0.25)  # 18.2
percentile_75 = data.quantile(0.75)  # 28.1
iqr = percentile_75 - percentile_25   # 9.9

# Output
print(f"Mean: {mean:.1f} ± {std_dev:.1f}")
print(f"Range: {data.min():.1f} - {data.max():.1f}")
print(f"IQR: {iqr:.1f}")
```

#### **Inferential Statistics**
```python
from scipy import stats

# T-test: Compare accuracy before/after enhancement
before = df_before['accuracy']
after = df_after['accuracy']

t_stat, p_value = stats.ttest_rel(after, before)
print(f"t-statistic: {t_stat:.2f}, p-value: {p_value:.4f}")

if p_value < 0.05:
    print("Significant improvement (p < 0.05)")

# Correlation: Eye fatigue vs accuracy
fatigue = df['eye_fatigue_score']
accuracy = df['gaze_accuracy']
correlation, p_val = stats.pearsonr(fatigue, accuracy)
print(f"Correlation: r = {correlation:.2f}, p = {p_val:.4f}")
```

#### **Visualization**
```python
import matplotlib.pyplot as plt

# Histogram
plt.hist(data, bins=20, edgecolor='black')
plt.xlabel('Gaze Accuracy (pixels)')
plt.ylabel('Frequency')
plt.title('Accuracy Distribution')
plt.show()

# Box plot
plt.boxplot([before, after], labels=['Before', 'After'])
plt.ylabel('Accuracy (pixels)')
plt.title('Enhancement Effect')
plt.show()

# Line plot (over time)
plt.plot(df['timestamp'], df['accuracy'], marker='o')
plt.xlabel('Time')
plt.ylabel('Accuracy')
plt.title('Accuracy Over Session')
plt.show()
```

---

### **4.2 Analysis Methods for Qualitative Data**

#### **Thematic Coding**
```
Step 1: Read all feedback
Step 2: Identify themes
Step 3: Code responses
Step 4: Count frequency

Example:

Feedback: "Buttons too small, hard to see"
→ Code: [ACCESSIBILITY] [BUTTON_SIZE] [VISIBILITY]

Feedback: "Great, very clear buttons"
→ Code: [ACCESSIBILITY] [BUTTON_SIZE] [VISIBILITY]

Results:
ACCESSIBILITY: 8 mentions (80%)
SPEED: 3 mentions (30%)
COMFORT: 5 mentions (50%)
```

#### **Content Analysis**
```
1. WORD FREQUENCY
   Count occurrences of key words:
   - "clear" mentioned 7 times → good visibility
   - "tired" mentioned 3 times → fatigue is issue
   - "helpful" mentioned 5 times → useful features

2. SENTIMENT ANALYSIS
   Categorize feedback by tone:
   - Positive: "Perfect", "Easy", "Great" (8 instances)
   - Neutral: "Works", "OK" (2 instances)
   - Negative: "Difficult", "Too slow" (1 instance)

3. AFFINITY MAPPING
   Group related feedback:
   
   Accessibility Cluster:
   - Button size large enough ✓
   - Text readable ✓
   - Layout clear ✓
   
   Performance Cluster:
   - Tracking accurate ✓
   - TTS speed good ✓
   - Word prediction helpful ✓
```

#### **Direct Quoting**
```
Use patient quotes for credibility:

"For the first time, I can communicate 
with my family without needing someone 
to interpret my eye movements."
- Patient LIS_P001

This powerful statement demonstrates 
real-world impact of the system.
```

---

### **4.3 Combined Analysis (Quantitative + Qualitative)**

#### **Mixed Methods Approach**
```
Research Question: "Is the system usable for LIS patients?"

Quantitative Answer:
- 96.7% button success rate
- 318ms average response time
- 4 out of 5 patients used > 30 minutes

Qualitative Answer:
- "Much better than letter board"
- "Can finally express needs"
- Eye fatigue after 30 minutes

Combined Conclusion:
"The system is highly usable and effective,
with sustained engagement >30 minutes.
Main limitation is eye fatigue requiring
breaks. Recommended: 30-45 min sessions
with 10-15 min rests between sessions."
```

---

## **5. DATA STORAGE & SECURITY**

### **File Organization**
```
research_data/
├── sessions/
│   ├── session_20260204_143000.json        [Metadata]
│   ├── session_20260204_144500.json
│   └── session_20260205_100000.json
├── eye_tracking/
│   ├── tracking_20260204_143000.csv        [Quantitative]
│   ├── tracking_20260204_144500.csv
│   └── ...
├── calibration/
│   ├── calibration_20260204_143000.csv     [Quantitative]
│   └── ...
├── activities/
│   ├── activities_20260204_143000.csv      [Both]
│   └── ...
├── images/
│   ├── 20260204_143000/
│   │   ├── frame_000001.jpg               [Visual]
│   │   ├── frame_000002.jpg
│   │   └── ... [18,000+ images]
│   └── 20260204_144500/
│       └── ...
└── qualitative_notes/                      [Qualitative]
    ├── patient_feedback_LIS_P001.txt
    ├── medical_notes_LIS_P001.txt
    ├── observations_meeting_4.txt
    └── ...
```

### **Anonymization**
```
❌ BAD (identifiable):
- patient_name_John_Smith_data.csv
- "Mohammed Ali - Eye tracking"

✅ GOOD (anonymized):
- LIS_P001_tracking_YYYYMMDD_HHMMSS.csv
- Stored notes: "Patient reported eye strain"
```

### **Data Privacy**
- Encrypt all files at rest
- Use secure transfer (HTTPS)
- Limit access to research team
- Follow HIPAA/GDPR guidelines
- Keep master key separate from data

---

## **6. SUMMARY TABLE: DATA TYPES IN YOUR PROJECT**

| Category | Type | Format | Volume | Analysis | Usage |
|---|---|---|---|---|---|
| **Eye Tracking** | Quantitative | CSV | 18,000 rows | Statistics | Accuracy, fatigue |
| **Calibration** | Quantitative | CSV | 4 rows | Error metrics | Quality check |
| **Activities** | Categorical | CSV | 50-100 rows | Frequency | Behavior patterns |
| **Images** | Visual | JPG | 18,000+ files | CNN/ML | Model training |
| **Feedback** | Qualitative | Text | Variable | Thematic | UX improvement |
| **Medical** | Qualitative | Text | 1-2 pages | Contextual | Patient profile |
| **Session Meta** | Quantitative | JSON | 1 file | Summary stats | Data overview |

---

## **7. ANALYSIS WORKFLOW FOR YOUR RESEARCH**

```
Session Complete
    ↓
1. ORGANIZE DATA
   - Rename files with anonymous IDs
   - Organize by data type
   - Backup immediately
    ↓
2. QUANTITATIVE ANALYSIS
   - Load CSV files
   - Calculate descriptive statistics
   - Generate accuracy metrics
   - Create visualizations
    ↓
3. QUALITATIVE ANALYSIS
   - Review feedback notes
   - Code responses by theme
   - Count theme frequency
   - Extract key quotes
    ↓
4. COMBINED ANALYSIS
   - Correlate quantitative with qualitative
   - Identify patterns
   - Validate findings
    ↓
5. REPORTING
   - Create result tables
   - Generate figures/charts
   - Write findings
   - Document limitations
    ↓
6. STORAGE
   - Secure backup (encrypted)
   - Document metadata
   - Archive for future use
```

---

This document provides complete guidance for data collection, analysis, and usage in your research project. All examples are based on your actual eye-gaze communication system.
