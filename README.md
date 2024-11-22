# InteractivePlushie

The following code is my self-assigned final project, a challenge I took on to push my limits. I set out with the goal of applying the knowledge gained from my AP Computer Science class. Over the course of two months, I dedicated time to brainstorming, programming, and polishing this supplement. It not only allowed me to independently apply the knowledge I acquired but also served as a platform for improving my programming skills. I take immense pride in my creation and have hopes of remastering it in the future. 

In the code I've written, I've created an interactive virtual plushie with various emotions and reactions to user interactions. The plushie has different elements, such as eyes, horns, and a crown. Through mouse interactions, like clicking on specific parts of the plushie, users can evoke emotions, from happiness and blushing to sadness and protection. Additionally, there's a centerpiece jewel that, when interacted with, can affect the plushie's emotions. The code utilizes functions for different emotional states and keeps track of user interactions in a memory bank, influencing the plushie's responses. It's an engaging and dynamic representation of a virtual character with a range of emotions.


# Importing necessary libraries
from graphics import *
import random

class Plushie:
    def __init__(self):
        # Set up the initial background and variables
        self.memoryBank = [0]
        self.karmaScore = 0
        self.z = 0
        self.jewel_held = False

        self.app = App("Virtual Plushie", width=500, height=500)
        self.app.background = gradient('red', 'blue')
        self.setup_plushie()
        self.create_emotions()

    def setup_plushie(self):
        # Setting up all the components of the plushie, such as legs, body, etc.
        # All objects like Circle, Polygon, etc., are defined here
        self.body = self.create_body()
        self.face = self.create_face()
        self.eyes = self.create_eyes()
        self.arms = self.create_arms()
        self.ears = self.create_ears()
        self.crown = self.create_crown()
        self.hearts = self.create_hearts()

    def create_body(self):
        # Body creation logic
        body = Polygon(150, 210, 170, 290, 230, 290, 250, 210, fill='lightSteelBlue')
        return body

    def create_face(self):
        # Face creation logic
        face = Polygon(200, 225, 150, 200, 125, 165, 135, 125, 145, 100, 170, 85, 200, 80, 
                       250, 200, 275, 165, 265, 125, 255, 100, fill='white', border='black')
        return face

    def create_eyes(self):
        # Eyes creation logic
        left_eye = Circle(170, 150, 19, fill='chartreuse')
        right_eye = Circle(230, 150, 19, fill='red')
        return Group(left_eye, right_eye)

    def create_arms(self):
        # Arms creation logic
        left_arm = Polygon(150, 210, 135, 300, 185, 310, 175, 245, border='white')
        right_arm = Polygon(250, 210, 265, 300, 215, 310, 225, 245, fill='white', border='black')
        return Group(left_arm, right_arm)

    def create_ears(self):
        # Ears creation logic
        ear1 = Polygon(135, 125, 70, 125, 125, 165)
        ear2 = Polygon(133, 135, 90, 125, 70, 125, 125, 165, fill='white')
        ear3 = Polygon(265, 125, 330, 125, 275, 165, fill='white')
        ear4 = Polygon(265, 135, 310, 125, 330, 125, 275, 165)
        return Group(ear1, ear2, ear3, ear4)

    def create_crown(self):
        # Crown creation logic
        crown = Polygon(170, 85, 160, 50, 180, 65, 200, 40, 220, 65, 240, 50, 230, 85, fill='gold')
        return crown

    def create_hearts(self):
        # Hearts creation logic
        top_heart = Polygon(105, 45, 100, 35, 90, 30, 75, 30, 65, 40, 65, 55, 70, 65, 80, 75, 90, 85, 105, 95)
        return Group(top_heart)

    def emotional_update(self, mouseX, mouseY):
        reactions = 0
        interactions = 0
        for i in self.memoryBank:
            reactions += i
            interactions += 1
            if i >= 25:
                self.memoryBank.pop(0)
        self.karmaScore = reactions / interactions

        if self.karmaScore >= 3:
            self.ranboo_glad(mouseX, mouseY)
            self.hearts.opacity = 100
        elif self.karmaScore >= 0:
            self.ranboo_glad(mouseX, mouseY)
        elif self.karmaScore >= -5:
            self.ranboo_sad(mouseX, mouseY)

    def ranboo_glad(self, mouseX, mouseY):
        # Logic for Ranboo's happy emotion
        self.smile.opacity = 100
        self.frown.opacity = 0
        self.tears.opacity = 0

    def ranboo_sad(self, mouseX, mouseY):
        # Logic for Ranboo's sad emotion
        self.smile.opacity = 0
        self.frown.opacity = 100
        self.tears.opacity = 50

    def on_mouse_move(self, mouseX, mouseY):
        self.emotional_update(mouseX, mouseY)

    def on_mouse_drag(self, mouseX, mouseY):
        if self.jewel.contains(mouseX, mouseY):
            self.jewel_held = True
        if self.jewel_held:
            self.jewel.centerX = mouseX
            self.jewel.centerY = mouseY
            self.ranboo_sad(mouseX, mouseY)

    def on_mouse_release(self, mouseX, mouseY):
        self.jewel.centerX = 200
        self.jewel.centerY = 55
        if self.jewel_held:
            self.jewel_held = False
            self.memoryBank.append(-5)
            print("You mocked Ranboo, earning a penalty of -5.")

# Main program logic

def main():
    plushie = Plushie()
    plushie.app.run()

if __name__ == "__main__":
    main()
