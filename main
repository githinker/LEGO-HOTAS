from mindstorms import Hub
import vgamepad as vg
import time
from spikedev.sensor import DistanceSensor

PITCH = 36
ROLL = 28
YAW = 70

hub = Hub()
hub.display.show("SNAKE")

dist = DistanceSensor(hub.port.B)
dist.set_mode(4)

hub.port.A.motor.mode([(2, 0),(3,0)])

error_A = hub.port.A.motor.get()[1]

hub.port.A.motor.run_to_position(-error_A,50)

time.sleep(2)

hub.port.A.motor.brake()

gamepad = vg.VDS4Gamepad()

throttle = 0

Playing = True

while True :

    if hub.button.center.was_pressed():
            Playing = True

    while Playing :

        distance = dist.value()[0]
        if distance == None  :{}
        elif distance > 500 :
            throttle = 1.
            gamepad.right_trigger_float(throttle)
        elif distance > 0 :
            throttle = 1. - float(distance)/500
            gamepad.right_trigger_float(throttle)

        pitch, roll = hub.motion.yaw_pitch_roll() [1:]

        pitch = float(pitch) / PITCH
        if pitch > 1 :
            pitch = 1
        elif pitch < -1 :
            pitch = -1

        if roll > 0:
            roll = float(roll -180) / ROLL
        else :
            roll = float(roll +180) / ROLL
        if roll > 1 :
            roll = 1
        elif roll < -1 :
            roll = -1

        gamepad.left_joystick_float(roll,-pitch)

        yaw = (hub.port.A.motor.get()[0] + error_A) / YAW
        if yaw > 1:
            yaw = 1
        elif yaw < -1:
            yaw = -1
        gamepad.right_joystick_float(yaw, 0)

        gamepad.update()

        if hub.button.center.was_pressed():
            Playing = False
            gamepad.reset()
            gamepad.update()
