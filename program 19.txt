#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#define MAX 10
char productions[MAX][MAX];
int numProductions;
void findLeading(char nonTerminal, char leadingSet[]) 
{
    for (int i = 0; i < numProductions; i++) 
	{
        if (productions[i][0] == nonTerminal) 
		{
            if (!isupper(productions[i][2])) 
			{
                strncat(leadingSet, &productions[i][2], 1);
            } else {
                findLeading(productions[i][2], leadingSet);
            }
        }
    }
}
int main() 
{
    printf("Enter number of productions: ");
    scanf("%d", &numProductions);
    getchar();
    printf("Enter the productions (e.g., E=E+T):\n");
    for (int i = 0; i < numProductions; i++) 
	{
        fgets(productions[i], MAX, stdin);
        productions[i][strcspn(productions[i], "\n")] = 0; // Remove newline
    }
    char leadingSet[MAX] = "";
    for (int i = 0; i < numProductions; i++) 
	{
        char nonTerminal = productions[i][0];
        printf("LEADING(%c) = {", nonTerminal);
        findLeading(nonTerminal, leadingSet);
        for (int j = 0; j < strlen(leadingSet); j++) 
		{
            printf(" %c", leadingSet[j]);
        }
        printf(" }\n");
        memset(leadingSet, 0, sizeof(leadingSet));
    }
    return 0;
}
