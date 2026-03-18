# Football-Player-Tracking-and-Performance-Analysis-Using-Deep-Learning

## ⚙️ System Pipeline

This project processes a football match video and generates advanced analytics such as **player tracking, speed estimation, distance covered, and ball possession statistics**.

The complete system pipeline is shown below.

### 🔷 Overall Pipeline

```mermaid
flowchart TD

A[Input Football Video] --> B[Frame Extraction using OpenCV]
B --> C[Object Detection YOLO]
C --> D[Object Tracking ByteTrack]
D --> E[Team Classification K-Means]
E --> F[Ball Detection]
F --> G[Ball Interpolation]
G --> H[Player Ball Assignment]
H --> I[Camera Motion Estimation Optical Flow]
I --> J[Position Correction]
J --> K[Perspective Transformation]
K --> L[Speed & Distance Calculation]
L --> M[Match Statistics]
M --> N[Annotated Output Video]

---

## 🔶 Step-by-Step Pipeline

### 1️⃣ Input Video

The football match video is loaded and converted into frames for processing.
Video → Frames → Process each frame


Libraries used:

- OpenCV (`cv2`)

---

### 2️⃣ Object Detection (YOLO)

Detects the following objects:

- Players  
- Ball  
- Referees  
- Goalkeepers  

Model used:

- YOLOv5 / YOLOv8 (fine-tuned)

Output per frame:
Bounding box
Class ID
Confidence score

Example:
Player → [x1, y1, x2, y2]
Ball → [x1, y1, x2, y2]


Purpose:  
✔ Detect all important objects in the frame.

---

### 3️⃣ Object Tracking (ByteTrack)

Object detection does not maintain object identity across frames.  
Tracking solves this issue.

Example:
Frame 1 → Player #7
Frame 2 → Player #7
Frame 3 → Player #7

Used:

- ByteTrack (via Supervision library)

Output:
Track ID
Bounding box
Class


Purpose:  
✔ Track the same player throughout the video.

---

### 4️⃣ Team Classification (K-Means Clustering)

Goal: determine which team each player belongs to.

Method:

1. Crop player image  
2. Extract shirt region  
3. Apply K-Means clustering on colors  

Example:
Cluster 1 → Team A (White)
Cluster 2 → Team B (Green)


Process:
Crop player
↓
Extract shirt pixels
↓
KMeans (k = 2)
↓
Assign team


Purpose:  
✔ Separate players by team.

---

### 5️⃣ Ball Detection + Interpolation

Problem:  
The ball is sometimes not detected in certain frames.

Solution:  
Interpolation to estimate missing positions.

Example:
Frame 1 → Ball detected
Frame 2 → Missing
Frame 3 → Ball detected


Frame 2 is estimated using interpolation.

Tools used:

- Pandas interpolation

Purpose:  
✔ Maintain continuous ball tracking.

---

### 6️⃣ Player–Ball Assignment

Goal: identify which player currently possesses the ball.

Method:
distance(player, ball)

If distance is below a threshold:
player.has_ball = True


Purpose:  
✔ Detect ball possession.

---

### 7️⃣ Camera Motion Estimation (Optical Flow)

Problem:  
Camera movement causes false player movement estimation.

Solution:  
Estimate camera motion using optical flow.

Methods used:

- `cv2.goodFeaturesToTrack`
- Lucas–Kanade Optical Flow

Steps:
Detect corner points
Track movement between frames
Estimate camera shift

Output:
camera_dx
camera_dy


Purpose:  
✔ Remove camera movement effects.

---

### 8️⃣ Position Correction

Player positions are adjusted to compensate for camera movement.
real_position = detected_position − camera_motion


Purpose:  
✔ Obtain true player movement.

---

### 9️⃣ Perspective Transformation

Pixels do not represent real-world distances.

Solution:

Transform the football field view into a top-down perspective.
Trapezoid → Rectangle

Used:

- Homography
- `cv2.getPerspectiveTransform`

Output:
Player position in meters

Purpose:  
✔ Convert pixel coordinates into real-world measurements.

---

### 🔟 Speed & Distance Calculation

Using transformed player positions.

Distance formula:
d = √((x₂ - x₁)² + (y₂ - y₁)²)


