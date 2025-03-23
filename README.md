# IntroToProg-Python-Mod07

import json

# Constants
MENU: str = """---- Course Registration Program ----
  Select from the following menu:  
    1. Register a Student for a Course
    2. Show current data  
    3. Save data to a file
    4. Exit the program
----------------------------------------- """
FILE_NAME: str = "Enrollments.json"

# Variables
menu_choice: str = ""
students: list = []


class FileProcessor:
    """Processes file input and output."""

    @staticmethod
    def read_data_from_file(file_name: str, student_data: list):
        """Reads data from a JSON file and populates the student_data list."""
        try:
            with open(file_name, 'r') as file:
                data = json.load(file)
                for item in data:
                    student_data.append(Student(item['first_name'], item['last_name'], item['course_name']))
        except Exception as e:
            output_error_messages("Error reading from file.", e)

    @staticmethod
    def write_data_to_file(file_name: str, student_data: list):
        """Writes student data to a JSON file."""
        try:
            with open(file_name, 'w') as file:
                json_data = [{'first_name': student.first_name, 'last_name': student.last_name,
                              'course_name': student.course_name} for student in student_data]
                json.dump(json_data, file)
        except Exception as e:
            output_error_messages("Error writing to file.", e)


class IO:
    """Handles input and output operations."""

    @staticmethod
    def output_menu(menu: str):
        """Displays the menu."""
        print(menu)

    @staticmethod
    def input_menu_choice() -> str:
        """Gets the user's menu choice."""
        return input("Enter your choice: ")

    @staticmethod
    def output_student_courses(student_data: list):
        """Displays the list of students and their courses."""
        if not student_data:
            print("No student data available.")
            return
        for student in student_data:
            print(f"{student.first_name} {student.last_name} is registered for {student.course_name}.")

    @staticmethod
    def input_student_data() -> tuple:
        """Gets student data from the user."""
        first_name = input("Enter the student's first name: ")
        last_name = input("Enter the student's last name: ")
        course_name = input("Enter the course name: ")
        return first_name, last_name, course_name


class Person:
    """Represents a person."""

    def __init__(self, first_name: str = "", last_name: str = ""):
        self.first_name = first_name
        self.last_name = last_name


class Student(Person):
    """Represents a student."""

    def __init__(self, first_name: str = "", last_name: str = "", course_name: str = ""):
        super().__init__(first_name, last_name)
        self.course_name = course_name

    @property
    def first_name(self) -> str:
        return self._first_name

    @first_name.setter
    def first_name(self, value: str):
        if not value.strip():
            raise ValueError("First name cannot be empty.")
        self._first_name = value

    @property
    def last_name(self) -> str:
        return self._last_name

    @last_name.setter
    def last_name(self, value: str):
        if not value.strip():
            raise ValueError("Last name cannot be empty.")
        self._last_name = value

    @property
    def course_name(self) -> str:
        return self._course_name

    @course_name.setter
    def course_name(self, value: str):
        if not value.strip():
            raise ValueError("Course name cannot be empty.")
        self._course_name = value


def output_error_messages(message: str, error: Exception = None):
    """Outputs error messages."""
    print(f"Error: {message}")
    if error:
        print(f"Details: {error}")


# Main program logic
if __name__ == "__main__":
    FileProcessor.read_data_from_file(FILE_NAME, students)

    while True:
        IO.output_menu(MENU)
        menu_choice = IO.input_menu_choice()

        if menu_choice == '1':
            try:
                first_name, last_name, course_name = IO.input_student_data()
                student = Student(first_name, last_name, course_name)
                students.append(student)
            except ValueError as e:
                output_error_messages("Invalid input.", e)

        elif menu_choice == '2':
            IO.output_student_courses(students)

        elif menu_choice == '3':
            FileProcessor.write_data_to_file(FILE_NAME, students)

        elif menu_choice == '4':
            print("Exiting the program.")
            break

        else:
            print("Invalid choice. Please select a valid option.")
