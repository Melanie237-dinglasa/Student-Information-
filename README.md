

#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX 100 // to store 100 only students can save to file 
#define SUBJECTS 3 // to define only  3 subjects to store and save in file
#define FILENAME "students.txt"// Makita nimo ang Students information nga mabasa Ra😊

typedef struct { //  I use typedef struct to 
    int id;
    char name[50];
    float grades[SUBJECTS];
    float average;
} Student;

// Functions to declare

/*
void addStudent(Student *s[], int *count);
void editStudent(Student *s[], int count);
void deleteStudent(Student *s[], int *count);
void computeAverage(Student *s);
void saveToFile(Student *s[], int count);
void loadFromFile(Student *s[], int *count);
void displayStudents(Student *s[], int count);
*/


/*addStudent 
Accepts an array of students and the current count (passed by pointer).
Takes input for ID, name, and grades.
Calls computeAverage() to calculate and store the average.
Increments the count after adding.
*/
void addStudent(Student students[], int *count) {
    Student *s = &students[*count];
    printf("\nEnter ID: ");
    scanf("%d", &s->id);
    printf("Enter a Name: ");
    getchar(); // to clear newline
    fgets(s->name, sizeof(s->name), stdin);
    s->name[strcspn(s->name, "\n")] = '\0';

    printf("Enter %d grades: ", SUBJECTS);
    for(int i = 0; i < SUBJECTS; i++) {
        scanf("%f", &s->grades[i]);
    }

    computeAverage(s);
    (*count)++;
    printf("Student added successfully!\n");
}

/* Editing student information 😊
When you choose option 2:
It asks for the student ID.
If found, you can change the name and grades.
Then it recomputes the average.
Changes are saved in memory, and later to file if you choose "Save and Exit".
*/

void editStudent(Student students[], int count) {
    int id, found = 0;
    printf("\nEnter student ID to edit: ");
    scanf("%d", &id);

    for(int i = 0; i < count; i++) {
        if(students[i].id == id) {
            printf("Editing %s\n", students[i].name);
            printf("Enter new name: ");
            getchar();
            fgets(students[i].name, sizeof(students[i].name), stdin);
            students[i].name[strcspn(students[i].name, "\n")] = '\0';

            printf("Enter %d new grades: ", SUBJECTS);
            for(int j = 0; j < SUBJECTS; j++) {
                scanf("%f", &students[i].grades[j]);
            }

            computeAverage(&students[i]);
            printf("Student updated successfully!\n");
            found = 1;
            break;
        }
    }

    if(!found)
        printf("Student with ID %d not found.\n", id);
       
}

/*deleteStudent
When you choose option 3:
It asks for the student ID to delete.
If found, that student is removed by shifting the remaining students.
Count is updated.
Will also be saved to file after you exit and save.
*/
void deleteStudent(Student students[], int *count) {
    int id, found = 0;
    printf("\nEnter student ID to delete: ");
    scanf("%d", &id);

    for(int i = 0; i < *count; i++) {
        if(students[i].id == id) {
            for(int j = i; j < *count - 1; j++) {
                students[j] = students[j + 1];
            }
            (*count)--;
            printf("Student deleted successfully!\n");
            found = 1;
            break;
        }
    }

    if(!found)
        printf("Student with ID %d not found.\n", id);
}
/*
displayStudent
Prints all students in a table format.
Shows ID, name, grades, and average.*/

void displayStudents(Student students[], int count) {
    printf("\n%-5s %-20s %-20s %-10s\n", "ID", "Name", "Grades", "Average");
    printf("--------------------------------------------------------------\n");
    for(int i = 0; i < count; i++) {
        printf("%-5d %-20s ", students[i].id, students[i].name);
        for(int j = 0; j < SUBJECTS; j++) {
            printf("%.1f ", students[i].grades[j]);
        }
        printf("   %.2f\n", students[i].average);
    }
}
/*computeAverage
Uses a pointer to a Student struct.
Loops through the grades and computes the average.
Stores the average directly in the struct.
*/
void computeAverage(Student *s) {
    float sum = 0;
    for(int i = 0; i < SUBJECTS; i++) {
        sum += s->grades[i];
    }
    s->average = sum / SUBJECTS;
}

/* It writes all current students to savetoFile in ("students.txt")
So, all changes—added, edited, deleted—will be saved.

*/
void saveToFile(Student students[], int count) {
    FILE *fp = fopen(FILENAME, "w");
    if(fp == NULL) {
        printf("Error saving file!\n");
        return;
    }

    for(int i = 0; i < count; i++) {
        fprintf(fp, "%d|%s|", students[i].id, students[i].name);
        for(int j = 0; j < SUBJECTS; j++) {
            fprintf(fp, "%.1f", students[i].grades[j]);
            if (j < SUBJECTS - 1) fprintf(fp, ",");
        }
        fprintf(fp, "|%.2f\n", students[i].average);
    }

    fclose(fp);
}


/* Diring dapita sa loadFromFile is  to open a file in read mode
Read and parses each line into the structure 
And it updates the student array and count
*/
void loadFromFile(Student students[], int *count) {
    FILE *fp = fopen(FILENAME, "r");
    if(fp == NULL) return;

    while(fscanf(fp, "%d|%[^|]|", &students[*count].id, students[*count].name) == 2) {
        for(int i = 0; i < SUBJECTS; i++) {
            fscanf(fp, "%f", &students[*count].grades[i]);
            if (i < SUBJECTS - 1) fscanf(fp, ",");
        }
        fscanf(fp, "|%f\n", &students[*count].average);
        (*count)++;
    }

    fclose(fp); // Is to close a file
}




int main() {
    Student students[MAX];
    int count = 0, choice;
  // many kulang pa po....
    loadFromFile(students, &count);

    return 0;
}
// Daghan pakung kulang ari sir hehe

}
