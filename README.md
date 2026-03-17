# Football-Player-Tracking-and-Performance-Analysis-Using-Deep-Learning

## 📊 System Pipeline Diagram

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
