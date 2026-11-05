#include <stdio.h>
#include <string.h>

#define MAX_STUDENTS 5
#define MAX_NAME_LEN 50
#define NUM_SUBJECTS 4

// Structure to store student information
struct Student {
    int id;
    char name[MAX_NAME_LEN];
    float marks[NUM_SUBJECTS];
    float average;
    char grade;
};

// Function prototypes
void inputStudents(struct Student students[], int size);
void calculatePerformance(struct Student students[], int size);
void displayAllStudents(const struct Student students[], int size);
void displayClassStatistics(const struct Student students[], int size);

int main() {
    struct Student students[MAX_STUDENTS];

    printf("=== STUDENT GRADE MANAGEMENT SYSTEM ===\n");
    
    // 1. Input student details
    inputStudents(students, MAX_STUDENTS);

    // 2. Process grades and averages
    calculatePerformance(students, MAX_STUDENTS);

    // 3. Display the report card table
    displayAllStudents(students, MAX_STUDENTS);

    // 4. Display overall statistics
    displayClassStatistics(students, MAX_STUDENTS);

    return 0;
}

// Function to input data for all students
void inputStudents(struct Student students[], int size) {
    for (int i = 0; i < size; i++) {
        printf("\nEnter details for Student %d:\n", i + 1);
        
        printf("Enter ID: ");
        scanf("%d", &students[i].id);
        
        // Clear input buffer
        while (getchar() != '\n'); 

        printf("Enter Name: ");
        fgets(students[i].name, MAX_NAME_LEN, stdin);
        // Remove trailing newline character from fgets if present
        students[i].name[strcspn(students[i].name, "\n")] = 0;

        printf("Enter marks for %d subjects (0 - 100):\n", NUM_SUBJECTS);
        for (int j = 0; j < NUM_SUBJECTS; j++) {
            do {
                printf("  Subject %d: ", j + 1);
                scanf("%f", &students[i].marks[j]);
                if (students[i].marks[j] < 0 || students[i].marks[j] > 100) {
                    printf("  Invalid marks! Please enter between 0 and 100.\n");
                }
            } while (students[i].marks[j] < 0 || students[i].marks[j] > 100);
        }
    }
}

// Function to calculate average and assign letter grades using arrays of marks
void calculatePerformance(struct Student students[], int size) {
    for (int i = 0; i < size; i++) {
        float sum = 0.0;
        for (int j = 0; j < NUM_SUBJECTS; j++) {
            sum += students[i].marks[j];
        }
        
        // Calculate average
        students[i].average = sum / NUM_SUBJECTS;

        // Assign grade based on average
        if (students[i].average >= 90) {
            students[i].grade = 'A';
        } else if (students[i].average >= 80) {
            students[i].grade = 'B';
        } else if (students[i].average >= 70) {
            students[i].grade = 'C';
        } else if (students[i].average >= 60) {
            students[i].grade = 'D';
        } else {
            students[i].grade = 'F';
        }
    }
}

// Function to display student records in a tabular format
void displayAllStudents(const struct Student students[], int size) {
    printf("\n\n=================================================================\n");
    printf("                        STUDENT REPORT CARD                      \n");
    printf("=================================================================\n");
    printf("%-5s | %-15s | %-8s | %-8s | %-6s\n", "ID", "Name", "Average", "Grade", "Status");
    printf("-----------------------------------------------------------------\n");
    
    for (int i = 0; i < size; i++) {
        printf("%-5d | %-15s | %-8.2f | %-8c | %-6s\n", 
            students[i].id, 
            students[i].name, 
            students[i].average, 
            students[i].grade,
            (students[i].grade == 'F') ? "Fail" : "Pass"
        );
    }
    printf("=================================================================\n");
}

// Function to find the class topper and overall class average
void displayClassStatistics(const struct Student students[], int size) {
    if (size <= 0) return;

    int topperIndex = 0;
    float classSum = 0.0;

    for (int i = 0; i < size; i++) {
        classSum += students[i].average;
        if (students[i].average > students[topperIndex].average) {
            topperIndex = i;
        }
    }

    float classAverage = classSum / size;

    printf("\nCLASS STATISTICS:\n");
    printf("- Overall Class Average: %.2f\n", classAverage);
    printf("- Class Topper: %s (ID: %d) with an Average of %.2f (Grade: %c)\n", 
        students[topperIndex].name, 
        students[topperIndex].id, 
        students[topperIndex].average, 
        students[topperIndex].grade
    );
}
