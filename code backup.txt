import turtle
import random

# Main menu
def start_game():
    wn.onkeypress(None, "a")  # Unbind 'a' key
    score_turtle.clear()
    score_turtle.goto(-380, 220)
    score_turtle.write("Score: 0", font=("Arial", 16, "normal"))
    main_menu_turtle.clear()
    main_menu_turtle.hideturtle()

# Screen
wn = turtle.Screen()
wn.setup(width=800, height=500)
wn.bgcolor('gray')
wn.tracer(2)

# Draw player
player = turtle.Turtle()
player.shape('triangle')
player.color('white')
player.penup()

# Create score turtle
score_turtle = turtle.Turtle()
score_turtle.penup()
score_turtle.color('white')
score_turtle.hideturtle()

# Create main menu turtle
main_menu_turtle = turtle.Turtle()
main_menu_turtle.penup()
main_menu_turtle.color('white')
main_menu_turtle.hideturtle()
main_menu_turtle.goto(-250, 0)
main_menu_turtle.write("Press 'a' to start the game\nLeft to move left, Right to move right, Space to shoot", font=("Arial", 16, "normal"))

# Game made by attribution
made_by_turtle = turtle.Turtle()
made_by_turtle.penup()
made_by_turtle.color('white')
made_by_turtle.hideturtle()
made_by_turtle.goto(-380, -220)
made_by_turtle.write("Game made by Omar Qawasmeh|Teacher: Ammar zalloum", font=("Arial", 12, "normal"))

# Create multiple goals and obstacles
goals = []
obstacles = []
for _ in range(5):
    goal = turtle.Turtle()
    goal.shape('circle')
    goal.color('red')
    goal.penup()
    goal.setposition(random.randint(-370, 370), random.randint(-230, 230))
    goals.append(goal)

for _ in range(3):
    obstacle = turtle.Turtle()
    obstacle.shape('circle')
    obstacle.color('orange')  # Choose obstacle color
    obstacle.penup()
    obstacle.setposition(random.randint(-370, 370), random.randint(-230, 230))
    obstacles.append(obstacle)

# Initialize score
score = 0

# Player movement
def turn_right():
    player.right(5)

def turn_left():
    player.left(5)

def shoot():
    global score
    # Create bullet
    bullet = turtle.Turtle()
    bullet.shape('circle')
    bullet.shapesize(.5)
    bullet.color('black')  # Change bullet color to black
    bullet.penup()
    bullet.setheading(player.heading())  # Set bullet direction to player's heading
    bullet.setposition(player.position()) # Start bullet from player's position
    bullet.speed(2)
    
    start_x, start_y = bullet.position()  # Get starting position of the bullet
    
    # Move bullet forward
    while True:  # Infinite loop to continuously move the bullet
        bullet.forward(1)
        # Check if the bullet has reached the edge of the screen
        if (abs(bullet.xcor() - start_x) > wn.window_width() / 2) or (abs(bullet.ycor() - start_y) > wn.window_height() / 2):
            bullet.hideturtle()  # Hide the bullet if it reaches the edge of the screen
            return
        # Check for collision with goals
        for goal in goals:
            if bullet.distance(goal) < 20:
                goal.hideturtle()  # Hide the goal if hit
                goals.remove(goal)  # Remove the goal from the list
                bullet.hideturtle()  # Hide the bullet
                score += 1  # Increment the score
                score_turtle.clear()  # Clear previous score
                score_turtle.write("Score: {}".format(score), font=("Arial", 16, "normal"))  # Update score display
                # Respawn the goal
                goal.setposition(random.randint(-400, 400), random.randint(-250, 250))
                goal.showturtle()  # Show the respawned goal
                goals.append(goal)  # Add the respawned goal to the list
                return
        # Check for collision with obstacles
        for obstacle in obstacles:
            if bullet.distance(obstacle) < 20:
                if obstacle.color()[0] == 'orange':
                    obstacle.hideturtle()  # Hide the obstacle if hit
                    obstacles.remove(obstacle)  # Remove the obstacle from the list
                    bullet.hideturtle()  # Hide the bullet
                    score -= 1  # Decrement the score
                    score_turtle.clear()  # Clear previous score
                    score_turtle.write("Score: {}".format(score), font=("Arial", 16, "normal"))  # Update score display
                    if score <= 0:
                        score_turtle.clear()  # Clear the score display
                        score_turtle.goto(-150, 0)
                        score_turtle.write("You lost!", font=("Arial", 20, "normal"))  # Display "You lost" message
                        # Respawn obstacles
                        for obstacle in obstacles:
                            obstacle.setposition(random.randint(-370, 370), random.randint(-230, 230))
                            obstacle.showturtle()
                        return  # Return after respawning obstacles
                    # Respawn the obstacle
                    obstacle.setposition(random.randint(-370, 370), random.randint(-230, 230))
                    obstacle.showturtle()  # Show the respawned obstacle
                    obstacles.append(obstacle)  # Add the respawned obstacle to the list
                    return
                else:
                    return

# Keybinds
wn.listen()
wn.onkeypress(turn_left, "Left")
wn.onkeypress(turn_right, "Right")
wn.onkey(shoot, "space")

# Key binding for starting the game
wn.onkeypress(start_game, "a")

wn.mainloop()
