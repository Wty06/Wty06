Description

The updated version of this program introduces several significant improvements. The original program was linear and asked the same questions in a fixed order, with simple scoring. In contrast, the new version randomizes the selection of questions using `random.randint()`, ensuring that each playthrough is unique. It also adds point deductions for incorrect answers and implements more complex scoring for questions with multiple parts, such as the conjugation of the verb "ser" and the natural ways to say "how are you" in Spanish. Functions were introduced for each question to modularize the code, making it more efficient and reusable. Additionally, the updated version calculates the player's score dynamically based on the actual points assigned to each question, reflecting a more flexible and accurate scoring system.

Here’s a detailed breakdown of the code to help you understand how each function works and its purpose on a fundamental level:

### 1. **Game Setup**
At the start of the game, the player is greeted, and asked if they want to play:

```python
print("Welcome to my Spanish quiz!")
playing = input("Would you like to play Tyler's Spanish Quiz? ")
if playing.lower() != "yes":
    quit()
```

- **`playing` input**: Asks the user if they want to play. If they don’t enter "yes" (case-insensitive), the game exits using `quit()`.

### 2. **Score System and Instructions**
The game initializes the player's score to 0 and explains how the game works:

```python
score = 0
print("""
Welcome to the game! You will be asked a series of questions. Each question is worth points, 
so be sure to answer as best as you can! At the end of the game, your total score and percentage 
will be calculated. Good luck!
""")
```

- **Score tracking**: The player's score is stored in the `score` variable, which starts at 0.
- **Explanation**: A multi-line string (`"""..."""`) explains the game rules.

### 3. **Individual Question Functions**
Each function below corresponds to a different question in the game. Functions are useful for reusing and organizing code.

#### **Question 1: Translation of 'Casa'**

```python
def question_1():
    answer = input("Translate the word 'Casa' to its English counterpart: ")
    if answer.lower() == "house":
        print("Correcto!")
        return 1
    else:
        print("Incorrecto")
        return 0
```

- **Input**: The player is asked to translate "Casa" into English.
- **Logic**: If the answer is "house", 1 point is awarded (`return 1`); otherwise, no points are awarded (`return 0`).

#### **Question 2: Translation of 'Door'**

```python
def question_2():
    answer = input("How do you say 'Door' in Spanish? ")
    if answer.lower() == "puerta":
        print("Correcto!")
        return 1
    else:
        print("Incorrecto")
        return 0
```

- Similar to **question_1**, this checks if the player can translate "door" to "puerta". One point is awarded for a correct answer.

#### **Question 3: Natural Ways to Say 'How Are You'**
This is the complex question where the player can provide up to three correct responses, and they lose points for incorrect/missing answers.

```python
def question_3():
    print("""
    You can provide up to 3 correct answers for how to naturally say 'how are you' in Spanish.
    Each correct answer is worth 1 point. If you provide fewer than 3 correct answers or give an incorrect answer,
    you'll lose 1 point for each missing or incorrect one.
    """)
    answer = input("What is a natural way to say 'how are you' in Spanish? (You can provide up to 3 answers, separated by commas): ")
    correct_answers = ['que tal', 'cómo estás', 'cómo está usted']
    user_answers = answer.lower().split(", ")
    points = 3  # Start with 3 points
    for ans in correct_answers:
        if ans not in user_answers:
            points -= 1  # Subtract 1 point for incorrect/missing answers
    if points == 3:
        print("Correcto!")
    elif points > 0:
        print(f"You got {points} correct answers.")
    else:
        print("Incorrecto! You got all answers wrong.")
    return points
```

- **Input**: The player provides up to 3 answers.
- **Logic**:
  - Correct answers are in `correct_answers`.
  - The player's input is split by commas to form a list.
  - For every missing or incorrect answer, 1 point is deducted.
  - The number of correct answers determines the points returned.

#### **Question 4: Verb Conjugation 'Ser'**
This question asks the player to conjugate the verb "ser."

```python
def question_4():
    answer = input("Conjugate the verb 'ser' in the present tense (write all forms separated by commas): ")
    conjugations = ["soy", "eres", "es", "somos", "sois", "son"]
    user_answer = answer.lower().split(", ")
    points = 6
    for i, correct_answer in enumerate(conjugations):
        if i < len(user_answer) and user_answer[i] != correct_answer:
            points -= 1  # Subtract 1 point for each incorrect form
    if points == 6:
        print("Correcto!")
    else:
        print(f"Incorrecto! You lost {6 - points} points for incorrect conjugations.")
    return points
```

- **Logic**:
  - The function compares the player’s conjugations with the correct list.
  - 1 point is deducted for each incorrect conjugation.
  - 6 points total if all forms are correct.

#### **Other Questions**
For the remaining questions, the logic is similar to **question_1** and **question_2**. Each function asks for input and checks if the player's answer matches the correct one. The number of points awarded depends on the question.

#### Example: **Question 9** asks for "little" in Spanish.

```python
def question_9():
    answer = input("How do you say 'little' in Spanish? ")
    if answer.lower() == "niña":
        print("Correcto!")
        return 10  # Worth 10 points
    else:
        print("Incorrecto")
        return 0
```

### 4. **Randomized Question Selection**
The game randomly selects and asks a subset of questions. The `random.randint()` function is used to avoid duplicates.

```python
# Create a list of the questions
questions = [question_1, question_2, question_3, question_4, question_5, question_6, question_7, question_8, question_9, question_10]

selected_questions = set()

# Randomly choose numbers for the questions and ensure no duplicates
while len(selected_questions) < 7:
    question_num = random.randint(0, 9)
    if question_num not in selected_questions:
        selected_questions.add(question_num)
        score += questions[question_num]()  # Call the corresponding question function
```

- **`selected_questions`**: Stores the question numbers that have already been asked to avoid duplicates.
- **`random.randint(0, 9)`**: Randomly picks a number between 0 and 9 to select a question.
- **Loop**: The `while` loop ensures that 7 unique questions are asked.

### 5. **Score Calculation**
After the player answers all the questions, their total score and percentage are calculated.

```python
total_possible_points = 38
percentage = (score / total_possible_points) * 100
print(f"Your score is: {percentage:.2f}%")
```

- **`total_possible_points`**: The sum of all points available for the 10 questions.
- **`percentage`**: The player's score is divided by the total possible points and multiplied by 100 to get the percentage.
- **`print()`**: Displays the final score as a percentage with two decimal places.

---

### Summary:
- **Input Handling**: Each question collects user input and compares it to predefined correct answers.
- **Randomization**: Questions are chosen randomly from a pool of 10, ensuring variety each game.
- **Score System**: Points are awarded or deducted based on correct/incorrect answers, and the final percentage is calculated.
- **Modular Functions**: Each question is separated into its own function, making the code organized and reusable.

This breakdown should help you understand each part of the code step-by-step!
