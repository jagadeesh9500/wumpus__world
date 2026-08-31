# Exp No. 9 — Solve Wumpus World Problem using Python Demonstrating Inferences from Propositional Logic
## Name : JAGADEESH P
## Register Number : 212223230083
### Aim

To solve the Wumpus World problem using Python by demonstrating inferences from propositional logic. The program allows an agent to move through a 4 × 4 Wumpus World, detect hazards using percepts such as breeze and stench, use an arrow to kill the Wumpus, collect gold, and avoid pits.
### Problem Statement

The Wumpus World is a 4 × 4 grid-based environment in which an intelligent agent must navigate from the starting position to the gold while avoiding dangerous pits and the Wumpus. The agent receives percepts such as **Breeze** near a pit and **Stench/Smell** near the Wumpus. Using these percepts, the agent must infer the possible locations of hazards using propositional logic and choose safe movements.

The agent can move **Up, Down, Left,** and **Right** and has an arrow that can be used to kill the Wumpus. The agent gains points by finding the gold, loses points for falling into a pit or encountering the Wumpus, and receives a penalty if the arrow is wasted.

The problem is to implement this Wumpus World environment in **Python** and demonstrate how an intelligent agent can use percepts and logical inferences to navigate the world safely and reach the gold.

<img width="433" height="328" alt="image" src="https://github.com/user-attachments/assets/59501b51-1af3-4a29-bf04-aed5d9ad8169" />

### Algorithm / Procedure

1. Initialise a 4 × 4 Wumpus World containing safe cells, pits, the Wumpus, and gold.
2. Initialise the agent's position at the top-left cell `[0,0]`.
3. Set the initial score to `0` and provide the agent with one arrow.
4. Display the available movements: Up, Down, Left and Right.
5. Accept the movement choice from the user.
6. Check whether the selected movement is within the boundaries of the 4 × 4 grid.
7. Move the agent to the selected adjacent cell if the movement is valid.
8. Check the contents/percepts of the current cell.
9. If the cell contains `Smell`, infer that the Wumpus may be in an adjacent cell.
10. If the agent has an arrow, ask whether the arrow should be fired.
11. Check the selected direction and determine whether the Wumpus is present in that adjacent cell.
12. If the Wumpus is hit, kill it, increase the score by 1000, and mark the Wumpus-related cells as safe.
13. If the arrow misses, decrease the score by 10 and consume the arrow.
14. If the agent enters the Wumpus cell, the agent dies and the score is reduced by 1000.
15. If the agent enters a pit, the agent falls into the pit, dies, and the score is reduced by 1000.
16. If the agent reaches the gold cell, increase the score by 1000 and terminate the game successfully.
17. Repeat the process until the agent reaches the gold, falls into a pit, encounters the Wumpus, or exits the game.
18. Display the final result and score.

### Program / Code

```python
# Exp No. 9
# Wumpus World using Python
# Demonstrating Inferences from Propositional Logic

wumpus = [
    ["Safe", "Breeze", "PIT", "Breeze"],
    ["Smell", "Safe", "Breeze", "Safe"],
    ["WUMPUS", "GOLD", "PIT", "Breeze"],
    ["Smell", "Safe", "Breeze", "PIT"]
]

# Initial variables
row = 0
column = 0
arrow = True
player = True
score = 0

print("====================================")
print("        WUMPUS WORLD")
print("====================================")
print("Starting position: [1,1]")
print("Initial score:", score)
print("Arrow available:", arrow)

while player:

    print("\n------------------------------------")
    print("Current position:", [row + 1, column + 1])
    print("Current percept:", wumpus[row][column])
    print("------------------------------------")

    choice = input(
        "Press u to move Up\n"
        "Press d to move Down\n"
        "Press l to move Left\n"
        "Press r to move Right\n"
        "Enter your choice: "
    ).lower()

    # Move Up
    if choice == "u":
        if row != 0:
            row -= 1
            print("Moved Up.")
        else:
            print("Move denied: boundary reached.")

    # Move Down
    elif choice == "d":
        if row != 3:
            row += 1
            print("Moved Down.")
        else:
            print("Move denied: boundary reached.")

    # Move Left
    elif choice == "l":
        if column != 0:
            column -= 1
            print("Moved Left.")
        else:
            print("Move denied: boundary reached.")

    # Move Right
    elif choice == "r":
        if column != 3:
            column += 1
            print("Moved Right.")
        else:
            print("Move denied: boundary reached.")

    else:
        print("Invalid choice. Move denied.")

    print("Current location:", wumpus[row][column])

    # Propositional inference using STENCH
    if wumpus[row][column] == "Smell" and arrow:

        print("\nInference: STENCH detected.")
        print("Therefore, Wumpus may be in an adjacent cell.")

        arrow_choice = input(
            "Do you want to throw the arrow?\n"
            "Press y to throw\n"
            "Press n to save the arrow: "
        ).lower()

        if arrow_choice == "y":

            arrow_throw = input(
                "Press u to throw Up\n"
                "Press d to throw Down\n"
                "Press l to throw Left\n"
                "Press r to throw Right\n"
                "Enter direction: "
            ).lower()

            target_row = row
            target_column = column

            # Determine target cell
            if arrow_throw == "u":
                target_row = row - 1

            elif arrow_throw == "d":
                target_row = row + 1

            elif arrow_throw == "l":
                target_column = column - 1

            elif arrow_throw == "r":
                target_column = column + 1

            else:
                print("Invalid arrow direction.")
                continue

            # Check whether target is inside the grid
            if (0 <= target_row < 4 and
                    0 <= target_column < 4):

                if wumpus[target_row][target_column] == "WUMPUS":

                    print("\nWumpus killed!")
                    score += 1000
                    arrow = False

                    print("Score:", score)

                    # Replace Wumpus with Safe
                    wumpus[target_row][target_column] = "Safe"

                    # Cells affected by Wumpus smell become safe
                    wumpus[1][0] = "Safe"
                    wumpus[3][0] = "Safe"

                    print("Inference: Wumpus has been eliminated.")
                    print("Adjacent dangerous percepts are updated.")

                else:
                    print("\nArrow wasted...")
                    score -= 10
                    arrow = False
                    print("Score:", score)

            else:
                print("Arrow cannot be fired outside the world.")
                score -= 10
                arrow = False
                print("Score:", score)

    # Check Wumpus
    if wumpus[row][column] == "WUMPUS":

        score -= 1000

        print("\n====================================")
        print("WUMPUS HERE!!")
        print("YOU DIE")
        print("Final score:", score)
        print("====================================")

        break

    # Check GOLD
    if wumpus[row][column] == "GOLD":

        score += 1000

        print("\n====================================")
        print("GOLD FOUND!")
        print("YOU WIN!")
        print("Final score:", score)
        print("====================================")

        player = False
        break

    # Check PIT
    if wumpus[row][column] == "PIT":

        score -= 1000

        print("\n====================================")
        print("Ahhhhh!!!!")
        print("You fell into a PIT.")
        print("YOU DIE")
        print("Final score:", score)
        print("====================================")

        break

print("\nGame Over.")
```

### Sample Output
<img width="640" height="612" alt="image" src="https://github.com/user-attachments/assets/c5174009-9f90-415b-998f-1938208a2841" />
<img width="604" height="418" alt="image" src="https://github.com/user-attachments/assets/4b966f0d-ef18-4536-a12b-7e731c24e090" />

### Output:
<img width="1726" height="437" alt="Screenshot 2026-08-25 113159" src="https://github.com/user-attachments/assets/fbfcf5a3-9731-4368-9400-019c4e59d343" />

<img width="1725" height="692" alt="image" src="https://github.com/user-attachments/assets/6feadd26-5cbd-4fd8-bf81-b0e9b9c28822" />

### Result:

Thus, the **Wumpus World problem was successfully implemented in Python** using percept-based reasoning and propositional-logic-style inference. The agent can move through the 4 × 4 world, identify possible hazards from **Breeze** and **Stench**, use an arrow to eliminate the Wumpus, avoid pits, collect the gold, and calculate the final score.
