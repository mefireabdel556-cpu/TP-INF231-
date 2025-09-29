# TP-INF231-
mefireabdel556@gmail.com
Suppression de toutes les occurrences d'un élément

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node* next;
} Node;

// Fonction pour créer un nouveau nœud
Node* createNode(int data) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Fonction pour supprimer toutes les occurrences
Node* deleteAllOccurrences(Node* head, int value) {
    Node* current = head;
    Node* prev = NULL;
    Node* temp;
    
    // Supprimer les occurrences en tête
    while (current != NULL && current->data == value) {
        temp = current;
        current = current->next;
        free(temp);
        head = current;
    }
    
    // Supprimer les occurrences dans le reste de la liste
    while (current != NULL) {
        if (current->data == value) {
            if (prev != NULL) {
                prev->next = current->next;
            }
            temp = current;
            current = current->next;
            free(temp);
        } else {
            prev = current;
            current = current->next;
        }
    }
    
    return head;
}

// Fonction pour afficher la liste
void displayList(Node* head) {
    Node* current = head;
    while (current != NULL) {
        printf("%d -> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}

int main() {
    Node* head = createNode(1);
    head->next = createNode(2);
    head->next->next = createNode(3);
    head->next->next->next = createNode(2);
    head->next->next->next->next = createNode(4);
    
    printf("Liste originale: ");
    displayList(head);
    
    head = deleteAllOccurrences(head, 2);
    
    printf("Après suppression des 2: ");
    displayList(head);
    
    return 0;
}
```

2. Insertion dans une liste simplement chaînée triée

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node* next;
} Node;

Node* createNode(int data) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Insertion dans une liste triée
Node* insertSorted(Node* head, int value) {
    Node* newNode = createNode(value);
    
    // Cas 1: Liste vide ou insertion en tête
    if (head == NULL || value < head->data) {
        newNode->next = head;
        return newNode;
    }
    
    // Cas 2: Insertion au milieu ou en queue
    Node* current = head;
    while (current->next != NULL && current->next->data < value) {
        current = current->next;
    }
    
    newNode->next = current->next;
    current->next = newNode;
    
    return head;
}

void displayList(Node* head) {
    Node* current = head;
    while (current != NULL) {
        printf("%d -> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}

int main() {
    Node* head = NULL;
    
    head = insertSorted(head, 5);
    head = insertSorted(head, 2);
    head = insertSorted(head, 8);
    head = insertSorted(head, 1);
    head = insertSorted(head, 4);
    
    printf("Liste triée: ");
    displayList(head);
    
    return 0;
}
```

3. Insertion dans une liste doublement chaînée triée

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct DNode {
    int data;
    struct DNode* prev;
    struct DNode* next;
} DNode;

DNode* createDNode(int data) {
    DNode* newNode = (DNode*)malloc(sizeof(DNode));
    newNode->data = data;
    newNode->prev = NULL;
    newNode->next = NULL;
    return newNode;
}

// Insertion dans une liste doublement chaînée triée
DNode* insertSortedDoubly(DNode* head, int value) {
    DNode* newNode = createDNode(value);
    
    // Cas 1: Liste vide
    if (head == NULL) {
        return newNode;
    }
    
    // Cas 2: Insertion en tête
    if (value < head->data) {
        newNode->next = head;
        head->prev = newNode;
        return newNode;
    }
    
    // Cas 3: Insertion au milieu ou en queue
    DNode* current = head;
    while (current->next != NULL && current->next->data < value) {
        current = current->next;
    }
    
    newNode->next = current->next;
    newNode->prev = current;
    
    if (current->next != NULL) {
        current->next->prev = newNode;
    }
    
    current->next = newNode;
    
    return head;
}

void displayDoublyList(DNode* head) {
    DNode* current = head;
    printf("NULL <-> ");
    while (current != NULL) {
        printf("%d <-> ", current->data);
        current = current->next;
    }
    printf("NULL\n");
}

int main() {
    DNode* head = NULL;
    
    head = insertSortedDoubly(head, 5);
    head = insertSortedDoubly(head, 2);
    head = insertSortedDoubly(head, 8);
    head = insertSortedDoubly(head, 1);
    head = insertSortedDoubly(head, 4);
    
    printf("Liste doublement chaînée triée: ");
    displayDoublyList(head);
    
    return 0;
}
```

4. Insertion dans une liste simplement chaînée circulaire

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int data;
    struct Node* next;
} Node;

Node* createNode(int data) {
    Node* newNode = (Node*)malloc(sizeof(Node));
    newNode->data = data;
    newNode->next = NULL;
    return newNode;
}

// Insertion en tête dans une liste circulaire
Node* insertAtHeadCircular(Node* last, int value) {
    Node* newNode = createNode(value);
    
    if (last == NULL) {
        // Liste vide
        newNode->next = newNode;
        return newNode;
    }
    
    newNode->next = last->next;
    last->next = newNode;
    return last;
}

// Insertion en queue dans une liste circulaire
Node* insertAtTailCircular(Node* last, int value) {
    Node* newNode = createNode(value);
    
    if (last == NULL) {
        newNode->next = newNode;
        return newNode;
    }
    
    newNode->next = last->next;
    last->next = newNode;
    return newNode; // Le nouveau nœud devient le dernier
}

void displayCircularList(Node* last) {
    if (last == NULL) {
        printf("Liste vide\n");
        return;
    }
    
    Node* current = last->next;
    
    do {
        printf("%d -> ", current->data);
        current = current->next;
    } while (current != last->next);
    
    printf("(retour au début)\n");
}

int main() {
    Node* last = NULL;
    
    printf("Insertion en tête:\n");
    last = insertAtHeadCircular(last, 3);
    last = insertAtHeadCircular(last, 2);
    last = insertAtHeadCircular(last, 1);
    displayCircularList(last);
    
    printf("Insertion en queue:\n");
    last = insertAtTailCircular(last, 4);
    last = insertAtTailCircular(last, 5);
    displayCircularList(last);
    
    return 0;
}
```

5. Insertion dans une liste doublement chaînée circulaire

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct DNode {
    int data;
    struct DNode* prev;
    struct DNode* next;
} DNode;

DNode* createDNode(int data) {
    DNode* newNode = (DNode*)malloc(sizeof(DNode));
    newNode->data = data;
    newNode->prev = NULL;
    newNode->next = NULL;
    return newNode;
}

// Insertion en tête dans une liste doublement circulaire
DNode* insertAtHeadDoublyCircular(DNode* head, int value) {
    DNode* newNode = createDNode(value);
    
    if (head == NULL) {
        // Liste vide
        newNode->next = newNode;
        newNode->prev = newNode;
        return newNode;
    }
    
    DNode* last = head->prev;
    
    newNode->next = head;
    newNode->prev = last;
    
    last->next = newNode;
    head->prev = newNode;
    
    return newNode; // Nouvelle tête
}

// Insertion en queue dans une liste doublement circulaire
DNode* insertAtTailDoublyCircular(DNode* head, int value) {
    DNode* newNode = createDNode(value);
    
    if (head == NULL) {
        newNode->next = newNode;
        newNode->prev = newNode;
        return newNode;
    }
    
    DNode* last = head->prev;
    
    newNode->next = head;
    newNode->prev = last;
    
    last->next = newNode;
    head->prev = newNode;
    
    return head; // La tête ne change pas
}

void displayDoublyCircularList(DNode* head) {
    if (head == NULL) {
        printf("Liste vide\n");
        return;
    }
    
    DNode* current = head;
    
    printf("↱ ");
    do {
        printf("%d <-> ", current->data);
        current = current->next;
    } while (current != head);
    printf("↴ (boucle)\n");
}

int main() {
    DNode* head = NULL;
    
    printf("Insertion en tête:\n");
    head = insertAtHeadDoublyCircular(head, 3);
    head = insertAtHeadDoublyCircular(head, 2);
    head = insertAtHeadDoublyCircular(head, 1);
    displayDoublyCircularList(head);
    
    printf("Insertion en queue:\n");
    head = insertAtTailDoublyCircular(head, 4);
    head = insertAtTailDoublyCircular(head, 5);
    displayDoublyCircularList(head);
    
    return 0;
}
