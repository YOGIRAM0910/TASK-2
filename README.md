# TASK-2

import tkinter as tk
from tkinter import messagebox
import random

class QuizApp:
    def __init__(self, master):
        self.master = master
        self.master.title("Quiz Application")
        self.master.geometry("400x300")
        self.questions = {
            "What is the capital of France?": ["Paris", "London", "Berlin", "Rome"],
            "What is the largest mammal?": ["Elephant", "Blue Whale", "Giraffe", "Lion"],
            "Who wrote 'Romeo and Juliet'?": ["William Shakespeare", "Charles Dickens", "Jane Austen", "F. Scott Fitzgerald"]
        }
        self.current_question = 0
        self.score = 0
        self.shuffle_questions()
        
        self.label_question = tk.Label(master, text="", font=("Arial", 12))
        self.label_question.pack(pady=10)
        
        self.radio_var = tk.IntVar()
        self.radio_buttons = []
        for i in range(4):
            rb = tk.Radiobutton(master, text="", variable=self.radio_var, value=i, font=("Arial", 10))
            self.radio_buttons.append(rb)
            rb.pack(pady=5, anchor="w")
        
        self.button_submit = tk.Button(master, text="Submit", command=self.submit_answer, font=("Arial", 10))
        self.button_submit.pack(pady=10)
        
        self.label_feedback = tk.Label(master, text="", font=("Arial", 10))
        self.label_feedback.pack()
        
        self.display_question()
    
    def shuffle_questions(self):
        # Shuffle the order of questions
        shuffled_questions = list(self.questions.items())
        random.shuffle(shuffled_questions)
        self.questions = dict(shuffled_questions)
    
    def display_question(self):
        question = list(self.questions.keys())[self.current_question]
        self.label_question.config(text=question)
        answers = self.questions[question]
        random.shuffle(answers)  # Shuffle the order of answers
        for i in range(4):
            self.radio_buttons[i].config(text=answers[i])
        self.radio_var.set(-1)
    
    def submit_answer(self):
        selected_index = self.radio_var.get()
        if selected_index == -1:
            messagebox.showerror("Error", "Please select an answer")
            return
        
        correct_answer = list(self.questions.values())[self.current_question][0]
        if self.radio_buttons[selected_index].cget("text") == correct_answer:
            self.score += 1
        
        self.current_question += 1
        if self.current_question < len(self.questions):
            self.display_question()
        else:
            self.show_results()
    
    def show_results(self):
        messagebox.showinfo("Results", f"You scored {self.score}/{len(self.questions)}")
        self.master.destroy()

def main():
    root = tk.Tk()
    app = QuizApp(root)
    root.mainloop()

if __name__ == "__main__":
    main()
