# String-Sorting-in-the-alphabetical-order

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_STR_LEN 20

//represents a node in a singly linked list
typedef struct link_node {
    char node_str[MAX_STR_LEN];
    struct link_node *next;
} node;

//declare the nodes and functions for sorting the list
int compare_node(struct link_node *n1, struct link_node *n2);
struct link_node *add_node(struct link_node    *list, struct link_node *node);
void display_list(struct link_node *head); //function to display list
void free_list(node *head);  //function to free list
void cleanInput(char *input);  //function to clean input


int main() {
    char input[MAX_STR_LEN];
    node *head, *newNode;   //declare nodes head and newNode
    head = NULL;
    printf("Enter a string\n");
    do {
        fgets(input, MAX_STR_LEN, stdin);// fgets stands for file get string.

        cleanInput(input);
        newNode = (node*)malloc(sizeof(node));  //allocate memory using mallloc function
        strcpy(newNode->node_str, input);
        newNode->next = NULL;   //make next point NULL
        if (input[0] != '\0') {
            head = add_node(head, newNode);
        }
    } while (input[0] != '\0');   //tranverse the list until input is the last node
    display_list(head);
    free_list(head);
    return 0;
}

struct link_node *add_node(struct link_node    *list, struct link_node *node){
    struct link_node *prev, *next;
    if (!list) {
        list = node;
    }
    else {
        prev = NULL;
        next = list;
        while (next && compare_node(node,next)>0) {
            prev = next;
            next = next->next;
        }
        if (!next) {
            prev->next = node;
        }
        else {
            if (prev) {
                node->next = prev->next;
                prev->next = node;
            }
            else {
                node->next = list;
                list = node;
            }
        }
    }
    return list;
}

void free_list(node *head) {
    node *prev = head;
    node *cur = head;
    while (cur) {
        prev = cur;
        cur = prev->next;
        free(prev);
    }
}
//code to compare the two nodes
int compare_node(struct link_node *n1, struct link_node *n2) {
    return strcmp(n1->node_str, n2->node_str);
}

void cleanInput(char* input) {
    int len = strlen(input);
    int i;
    for (i = 0; i < len-1; i++) {}
    input[i] = '\0';
}




//display() will display all the nodes present in the list
void display_list(struct link_node *head) {
    while (head) {
        printf("%s \n", head->node_str);
        head = head->next; //points the head node to the next node
    }
}
