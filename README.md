# Hand Gesture Control System

## Project Overview

This project implements a real-time **Hand Gesture Control System** using computer vision. It allows a user to interact with a computer by recognizing the number of extended fingers and mapping those counts to standard keyboard commands. This provides a touchless, alternative input method for applications like presentations or media control.

### Key Technologies

* **Python:** Core programming language.
* **OpenCV (`cv2`):** Handles video capture and display.
* **MediaPipe Hands:** Used for robust, real-time hand tracking and 21-point landmark detection.
* **`pyautogui`:** Used to simulate keyboard press events (e.g., 'right', 'space').

## Setup and Installation

### Prerequisites

* Python 3.12.7 installed.

### Installation Steps
1.  **Install Dependencies:** Install all required Python packages.
    ```bash
    pip install opencv-python mediapipe pyautogui
    ```

## Working

1.  **Run the script:**
    ```bash
    python main.py
    ```
2.  A webcam window named "window" will open. Position your hand clearly in front of the camera.
3.  Perform the gestures shown below to control your device.
4.  Press the **ESC** key to exit the application.

### Gesture-to-Action Mapping

| Finger Count | Gesture Description | Key Press Action | Common Use Case |
| :----------: | :--- | :--------: | :------- |
| 1 | Index finger up | `right` | Forward skip 5/10 seconds |
| 2 | Index and Middle up | `left` | Rewind 5/10 seconds |
| 3 | Index, Middle, Ring up | `up` | Volume up |
| 4 | All fingers except Thumb up | `down` |  Volume down |
| 5 | All five fingers up | `space` | Play/Pause |

## 🧠 Implementation Logic

The core of the project is the `count_fingers` function, which determines the number of extended fingers using specific hand landmarks .

1.  **Dynamic Thresholding:** The "up" status for each finger is determined by comparing the vertical distance (along the Y-axis) between the base joint and the tip of the finger against a dynamic threshold. This threshold is defined as half the vertical distance between the wrist (landmark 0) and the central MCP joint (landmark 9), which helps adapt the detection to the user's hand size and position.

The specific landmarks used for calculation are as follows:

a. Index Finger: The vertical distance between the 5th landmark (MCP joint/base) and the 8th landmark (Tip) is calculated.

b. Middle Finger: The vertical distance between the 9th landmark (MCP joint/base) and the 12th landmark (Tip) is calculated.

c. Ring Finger: The vertical distance between the 13th landmark (MCP joint/base) and the 16th landmark (Tip) is calculated.

d. Pinky Finger: The vertical distance between the 17th landmark (MCP joint/base) and the 20th landmark (Tip) is calculated.

2.  **Thumb Check:** The thumb's status is checked separately using a fixed **X-axis** distance threshold (5) between the **Thumb Tip (4)** and the **Index MCP Joint (5)**.
   
<img width="485" height="442" alt="landmarks" src="https://github.com/user-attachments/assets/924efe2f-245b-44e6-90ed-b89216967731" />
<img width="485" height="442" alt="threshold" src="https://github.com/user-attachments/assets/07f860da-bf24-406f-a54a-61c810e6cc6c" />
<img width="485" height="442" alt="y axis" src="https://github.com/user-attachments/assets/9576dfb0-22a1-47de-a7d3-b3aeb1a171ef" />
<img width="485" height="442" alt="x axis" src="https://github.com/user-attachments/assets/2e3e7f84-c2f0-425f-a625-328c61262cc4" />

### Addressing Stability Issues

The following two key solutions were implemented to ensure robust control:

| Challenge | Solution | Code Implementation |
| :--- | :--- | :--- |
| **Repetitive Output** | **State Management:** The `prev` variable ensures that a keypress is triggered only when the current count (`cnt`) is **changes** from the previous count. | `if not(prev == cnt ): ... prev = cnt` |
| **Gesture Flickering (Debouncing)** | **Time Delay:** A 0.2-second delay is introduced (`end_time - start_time > 0.2`) to ensure the detected finger count is stable before triggering a `pyautogui` action. This prevents unintentional rapid actions. | `if not(start_initial): ... elif(end_time - start_time) > 0.2:` |
