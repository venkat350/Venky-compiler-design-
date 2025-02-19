#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <ctype.h>
#define MAX 10
char productions[MAX][MAX];
int numProductions;
void findTrailing(char nonTerminal, char trailingSet[]) 
{
    for (int i = 0; i < numProductions; i++) 
	{
        int len = strlen(productions[i]);
        if (productions[i][0] == nonTerminal) 
		{
            if (!isupper(productions[i][len - 1])) 
			{
                strncat(trailingSet, &productions[i][len - 1], 1);
            } else {
                findTrailing(productions[i][len - 1], trailingSet);
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
        productions[i][strcspn(productions[i], "\n")] = 0;
    }
    char trailingSet[MAX] = "";
    for (int i = 0; i < numProductions; i++) 
	{
        char nonTerminal = productions[i][0];
        printf("TRAILING(%c) = {", nonTerminal);
        findTrailing(nonTerminal, trailingSet);
        for (int j = 0; j < strlen(trailingSet); j++) 
		{
            printf(" %c", trailingSet[j]);
        }
        printf(" }\n");
        memset(trailingSet, 0, sizeof(trailingSet));
    }
    return 0;
}
