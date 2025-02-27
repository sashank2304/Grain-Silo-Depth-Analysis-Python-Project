🌾 Silo Level Sensor  

## 📌 Objective  
To optimize inventory management, prevent overfilling or underfilling of silos, and minimize operational risks.  

## 🛠 Components Required  

| Sl. No | Component        | Quantity |
|--------|----------------|----------|
| 1      | Raspberry Pi    | 1        |
| 2      | Ultrasonic Sensor | 1        |
| 3      | Jumper Cables   | 4        |

## 🔌 Connections  

- **Trigger pin** of the ultrasonic sensor → GPIO 11 (Physical PIN 23) via level shifter  
- **Echo pin** of the ultrasonic sensor → GPIO 12 (Physical PIN 32)  
- **5V & GND** of the ultrasonic sensor → 5V & GND of Raspberry Pi  

## 💻 Code  

python
import RPi.GPIO as GPIO
import time

TRIG_PIN = 23
ECHO_PIN = 24
THRESHOLD_DISTANCE = 100

GPIO.setmode(GPIO.BCM)
GPIO.setup(TRIG_PIN, GPIO.OUT)
GPIO.setup(ECHO_PIN, GPIO.IN)

def measure_distance():
    GPIO.output(TRIG_PIN, False)
    time.sleep(0.1)
    GPIO.output(TRIG_PIN, True)
    time.sleep(0.00001)
    GPIO.output(TRIG_PIN, False)

    while GPIO.input(ECHO_PIN) == 0:
        pulse_start = time.time()
    while GPIO.input(ECHO_PIN) == 1:
        pulse_end = time.time()

    pulse_duration = pulse_end - pulse_start
    distance = pulse_duration * 17150
    return round(distance, 2)

try:
    while True:
        distance = measure_distance()
        print("Measured Distance:", distance, "cm")

        if distance > THRESHOLD_DISTANCE:
            print("Grain level is below the threshold. Take action!")
        
        time.sleep(1)

except KeyboardInterrupt:
    print("Exiting program")

finally:
    GPIO.cleanup()


## 📖 Code Explanation  

- The `measure_distance()` function sends a trigger pulse to the ultrasonic sensor and calculates the distance in cm.  
- The main loop continuously measures distance and prints it.  
- If the distance exceeds the threshold, a warning message is displayed.  

## ⚙️ Working Principle  

- An ultrasonic sensor emits sound pulses that bounce off the grain and return.  
- The Raspberry Pi calculates the time delay and determines the distance.  
- The filled percentage is derived using:  

  \[
  \text{Filled Percentage} = \left( 1 - \frac{\text{Measured Distance}}{\text{Total Silo Height}} \right) \times 100
  \]

## 📊 Observations  

| Level Filled | Sensor Value | Percentage Shown |
|-------------|-------------|------------------|
| 32 cm       | 31-32 cm    | 96-99%          |
| 26 cm       | 25-26 cm    | 78-81%          |
| 20 cm       | 19-21 cm    | 59-66%          |

## ✅ Conclusion  

- The ultrasonic sensor successfully detects silo grain levels with **96% accuracy**.  
- Enables remote monitoring and prevents inventory mismanagement.  

## 🚀 Future Improvements  

- 📡 **IoT Integration** – Send real-time data to cloud dashboards.  
- 📊 **Data Logging** – Maintain historical grain level trends.  
- 🔔 **Automated Alerts** – Notify users when levels drop below a threshold.  

---

💡 **Developed by**: Sashank Abburu  
📌 **License**: MIT  
  
