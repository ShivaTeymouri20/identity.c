#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#define MAX_RECORDS 100

typedef struct {
    char nationalID[11];
    char firstName[50];
    char lastName[50];
} Record;

void loadRecords(const char *filename, Record records[], int *recordCount) {
    FILE *file = fopen(filename, "r");
    if (!file) {
        perror("Error opening file");
        exit(EXIT_FAILURE);
    }

    *recordCount = 0;
    while (fscanf(file, "%s %s %s", records[*recordCount].nationalID,
                  records[*recordCount].firstName,
                  records[*recordCount].lastName) != EOF) {
        (*recordCount)++;
                  }

    fclose(file);
}

int main() {
    Record records[MAX_RECORDS];
    int recordCount;

    // Load data from the file
    loadRecords("data.txt", records, &recordCount);

    // Ask the user for a National ID
    char inputID[11];
    printf("Enter a National ID to guess the name: ");
    scanf("%s", inputID);

    // Search for the ID in the records
    int found = 0;
    for (int i = 0; i < recordCount; i++) {
        if (strcmp(records[i].nationalID, inputID) == 0) {
            printf("The name for National ID %s is: %s %s\n", inputID, records[i].firstName, records[i].lastName);
            found = 1;
            break;
        }
    }

    if (!found) {
        printf("No record found for the given National ID.\n");
    }

    return 0;
}
