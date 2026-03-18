### 🔷 Overall System Pipeline

```mermaid
flowchart LR

%% ================= INPUT =================
subgraph INPUT
A[Input Football Video]
B[Frame Extraction<br>OpenCV]
A --> B
end

%% ================= DETECTION =================
subgraph DETECTION
C[Object Detection<br>YOLO]
D[Object Tracking<br>ByteTrack]
E[Team Classification<br>K-Means]
F[Ball Detection]
G[Ball Interpolation]
end

B --> C
C --> D
D --> E
E --> F
F --> G

%% ================= ANALYSIS =================
subgraph ANALYSIS
H[Player-Ball Assignment]
I[Camera Motion Estimation<br>Optical Flow]
J[Position Correction]
K[Perspective Transform]
L[Speed & Distance]
M[Match Statistics]
end

G --> H
H --> I
I --> J
J --> K
K --> L
L --> M

%% ================= OUTPUT =================
subgraph OUTPUT
N[Annotated Video]
end

M --> N
