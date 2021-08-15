# githubTest Predictive Typing in Search Engine Using data Structures in C
#include <stdio.h>
#include <stdlib.h>
#include<string.h>
#include <stdbool.h>
#define ALPHABET_SIZE (26)
#define CHAR_TO_INDEX(c) ((int)c - (int)'a')

struct node
{
struct node *children[ALPHABET_SIZE];
bool isWordEnd;
};
struct node *getNode(void)
{
  struct node *pNode = NULL;

pNode = (struct node *)malloc(sizeof(struct node));

if (pNode)
{
int i;

pNode->isWordEnd = false;

for (i = 0; i < ALPHABET_SIZE; i++)
pNode->children[i] = NULL;
}

return pNode;
}

void insert(struct node *root, const char * key)
{
struct node *next = root;
int length =strlen(key);

for (int level = 0; level < length; level++)
{
int index = CHAR_TO_INDEX(key[level]);
if (!next->children[index])
next->children[index] = getNode();

next = next->children[index];
}
next->isWordEnd = true;
}
bool search(struct node *root,const char *key)
{
int length =strlen(key);
struct node *next = root;
for (int level = 0; level < length; level++)
{
int index = CHAR_TO_INDEX(key[level]);

if (!next->children[index])
return false;

next = next->children[index];
}

return (next != NULL && next->isWordEnd);
}
bool isLastNode(struct node* root)
{
for (int i = 0; i < ALPHABET_SIZE; i++)
if (root->children[i])
return 0;
return 1;
}
void suggestionsRec(struct node* root,const char * currPrefix)
{
// found a string in Trie with the given prefix
if (root->isWordEnd)
{
printf("%s",currPrefix);
}

// All children struct node pointers are NULL
if (isLastNode(root))
return;

for (int i = 0; i < ALPHABET_SIZE; i++)
{
if (root->children[i])
{

suggestionsRec(root->children[i], currPrefix);
}
}
}
 struct autocomplete(struct node* root,const char *b)
{
struct node* next = root;
int level;
int n = b.length();
for (level = 0; level < n; level++)
{
int index = CHAR_TO_INDEX(b[level]);
if (!next->children[index])
return 0;

next = next->children[index];
}
bool isWord = (next->isWordEnd == true);
bool isLast = isLastNode(next);

if (isWord && isLast)
{
   printf("%c",b);
return -1;
}
if (!isLast)
{
string prefix = b;
suggestionsRec(next, prefix);
return 1;
}
}
int main()
{
struct node* root = getNode();
insert(root, "hello");
insert(root, "dog");
insert(root, "hell");
insert(root, "cat");
insert(root, "a");
insert(root, "hel");
insert(root, "duck");
insert(root, "hen");
insert(root, "cattle");
insert(root, "asia");
insert(root, "hel");
insert(root, "help");
insert(root, "helps");
insert(root, "helping");
int r = autocomplete(root, "hel");
if (r == -1)
printf("No other strings found with these starting letters \n");
else if (r == 0)
printf( "No string found with these starting letters\n");
return 0;
}
