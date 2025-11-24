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
    python your_script_name.py
    ```
2.  A webcam window named "window" will open. Position your hand clearly in front of the camera.
3.  Perform the gestures shown below to control your device.
4.  Press the **ESC** key to exit the application.

### Gesture-to-Action Mapping

| Finger Count | Gesture Description | Key Press Action | Common Use Case |
| :----------: | :--- | :--------: | :------- |
| 1 | Index finger up | `right` | Next slide, Forward skip |
| 2 | Index and Middle up | `left` | Previous slide, Rewind |
| 3 | Index, Middle, Ring up | `up` | Scroll up, Volume up |
| 4 | All fingers except Thumb up | `down` | Scroll down, Volume down |
| 5 | All five fingers up | `space` | Play/Pause, Select |

## 🧠 Implementation Logic

The core of the project is the `count_fingers` function, which determines the number of extended fingers using specific hand landmarks .

1.  **Dynamic Thresholding:** For the Index, Middle, Ring, and Pinky fingers, the "up" status is determined by comparing the distance between the finger's **MCP joint** (base) and its **Tip** along the **Y-axis** against a calculated threshold. The threshold is defined as half the vertical distance between the wrist (0) and the central MCP joint (9). This dynamic approach adapts to hand size and camera distance.
2.  **Thumb Check:** The thumb's status is checked separately using a fixed **X-axis** distance threshold (5) between the **Thumb Tip (4)** and the **Index MCP Joint (5)**.

### Addressing Stability Issues

The following two key solutions were implemented to ensure robust control:

| Challenge | Solution | Code Implementation |
| :--- | :--- | :--- |
| **Repetitive Output** | **State Management:** The `prev` variable ensures that a keypress is triggered only when the current count (`cnt`) is **changes** from the previous count. | `if not(prev == cnt ): ... prev = cnt` |
| **Gesture Flickering (Debouncing)** | **Time Delay:** A 0.2-second delay is introduced (`end_time - start_time > 0.2`) to ensure the detected finger count is stable before triggering a `pyautogui` action. This prevents unintentional rapid actions. | `if not(start_initial): ... elif(end_time - start_time) > 0.2:` |
