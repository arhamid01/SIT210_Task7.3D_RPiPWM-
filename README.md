# SIT210_Task7.3D_RPiPWM-
import RPi.GPIO as GPIO
import time

GPIO.setmode(GPIO.BOARD)

TRIG = 16
ECHO = 18

GPIO.setup(TRIG,GPIO.OUT)
GPIO.setup(ECHO,GPIO.IN)

GPIO.output(TRIG, False)

print ("Calibrating.....")
time.sleep(2)

GPIO.setup(12, GPIO.OUT)
pwm = GPIO.PWM(12, 100)

print("\nPress Ctl C to quit \n")
dc=0
pwm.start(dc)
try:
    while True:
        GPIO.output(TRIG, True)
        time.sleep(0.00001)
        GPIO.output(TRIG, False)

        while GPIO.input(ECHO)==0:
            pulse_start = time.time()

        while GPIO.input(ECHO)==1:
            pulse_end = time.time()

        pulse_duration = pulse_end - pulse_start

        distance = pulse_duration * 17150

        distance = round(distance+1.15, 2)
        if distance <= 20: 
            print("distance:",distance,"cm")
            dc = 70
            pwm.ChangeDutyCycle(dc)
            time.sleep(0.05)
            print(dc)
            print("LED is very bright")
        if distance > 20 and distance < 100:
            print("distance:",distance,"cm")
            dc = 10
            pwm.ChangeDutyCycle(dc)
            time.sleep(0.05)
            print(dc)
            print("LED is not bright")
except KeyboardInterrupt:
    GPIO.cleanup()
