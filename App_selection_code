
import cv2
import mediapipe as mp
import webbrowser
import time

mp_hands = mp.solutions.hands
hands = mp_hands.Hands(static_image_mode=False, max_num_hands=2, min_detection_confidence=0.7)
mp_drawing = mp.solutions.drawing_utils

webcam = cv2.VideoCapture(0)

gestures = {
    2: 'https://www.youtube.com',
    3: 'https://www.google.com',
    4: 'https://web.whatsapp.com',
    5: 'https://www.linkedin.com',
    1: 'https://www.spotify.com',
}

current_gesture = None
is_site_open = False
cooldown_duration = 5  # Set the cooldown duration in seconds
last_open_time = 0

while True:
    _, image = webcam.read()
    image = cv2.flip(image, 1)
    image_rgb = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

    results = hands.process(image_rgb)

    if results.multi_hand_landmarks:
        for hand_landmarks in results.multi_hand_landmarks:
            mp_drawing.draw_landmarks(image, hand_landmarks, mp_hands.HAND_CONNECTIONS)

            landmarks = hand_landmarks.landmark

            thumb = landmarks[4].x < landmarks[3].x < landmarks[2].x < landmarks[1].x
            index_finger = landmarks[8].y < landmarks[6].y
            middle_finger = landmarks[12].y < landmarks[10].y
            ring_finger = landmarks[16].y < landmarks[14].y
            pinky_finger = landmarks[20].y < landmarks[18].y
            num_fingers = sum([thumb, index_finger, middle_finger, ring_finger, pinky_finger])

            cv2.putText(image, f"Fingers: {num_fingers}", (10, 30), cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 255, 0), 2)

            if num_fingers in gestures and (time.time() - last_open_time) > cooldown_duration:
                if current_gesture != num_fingers:
                    if is_site_open:
                        webbrowser.open('about:blank')
                        is_site_open = False

                    webbrowser.open(gestures[num_fingers])
                    current_gesture = num_fingers
                    is_site_open = True
                    last_open_time = time.time()

    cv2.imshow("Hand Gesture Detection", image)

    key = cv2.waitKey(1)
    if key == ord('q'):
        break

webcam.release()
cv2.destroyAllWindows()
