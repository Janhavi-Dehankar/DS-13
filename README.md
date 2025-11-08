# DS-13
DS code
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    struct Node *prev, *next;
};

struct Node* head = NULL;

struct Node* createNode(int x) {
    struct Node* newNode = (struct Node*)malloc(sizeof(struct Node));
    newNode->data = x;
    newNode->prev = newNode->next = NULL;
    return newNode;
}

void create(int n) {
    struct Node *temp, *newNode;
    int val;
    for (int i = 0; i < n; i++) {
        printf("Enter data: ");
        scanf("%d", &val);
        newNode = createNode(val);
        if (head == NULL)
            head = newNode;
        else {
            temp = head;
            while (temp->next != NULL)
                temp = temp->next;
            temp->next = newNode;
            newNode->prev = temp;
        }
    }
}

void insert(int pos, int val) {
    struct Node *newNode = createNode(val), *temp = head;
    if (pos == 1) {
        newNode->next = head;
        if (head) head->prev = newNode;
        head = newNode;
        return;
    }
    for (int i = 1; temp && i < pos - 1; i++)
        temp = temp->next;
    if (temp == NULL) return;
    newNode->next = temp->next;
    if (temp->next) temp->next->prev = newNode;
    temp->next = newNode;
    newNode->prev = temp;
}

void deleteNode(int pos) {
    struct Node* temp = head;
    if (head == NULL) return;
    if (pos == 1) {
        head = head->next;
        if (head) head->prev = NULL;
        free(temp);
        return;
    }
    for (int i = 1; temp && i < pos; i++)
        temp = temp->next;
    if (temp == NULL) return;
    if (temp->next) temp->next->prev = temp->prev;
    if (temp->prev) temp->prev->next = temp->next;
    free(temp);
}

void reverse() {
    struct Node *curr = head, *temp = NULL;
    while (curr) {
        temp = curr->prev;
        curr->prev = curr->next;
        curr->next = temp;
        curr = curr->prev;
    }
    if (temp) head = temp->prev;
}

void merge(struct Node* h2) {
    if (head == NULL) head = h2;
    else {
        struct Node* temp = head;
        while (temp->next) temp = temp->next;
        temp->next = h2;
        if (h2) h2->prev = temp;
    }
}

void middle() {
    struct Node *slow = head, *fast = head;
    if (head == NULL) return;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
    }
    printf("Middle element: %d\n", slow->data);
}

void sort() {
    struct Node *i, *j;
    int temp;
    for (i = head; i != NULL; i = i->next)
        for (j = i->next; j != NULL; j = j->next)
            if (i->data > j->data) {
                temp = i->data;
                i->data = j->data;
                j->data = temp;
            }
}

int detectLoop() {
    struct Node *slow = head, *fast = head;
    while (fast && fast->next) {
        slow = slow->next;
        fast = fast->next->next;
        if (slow == fast) return 1;
    }
    return 0;
}

void sumData() {
    int sum = 0;
    struct Node* temp = head;
    while (temp) {
        sum += temp->data;
        temp = temp->next;
    }
    printf("Sum of data: %d\n", sum);
}

void printOddEven() {
    struct Node* temp = head;
    printf("Odd data: ");
    while (temp) {
        if (temp->data % 2 != 0) printf("%d ", temp->data);
        temp = temp->next;
    }
    temp = head;
    printf("\nEven data: ");
    while (temp) {
        if (temp->data % 2 == 0) printf("%d ", temp->data);
        temp = temp->next;
    }
    printf("\n");
}

void display() {
    struct Node* temp = head;
    while (temp) {
        printf("%d ", temp->data);
        temp = temp->next;
    }
    printf("\n");
}

int main() {
    int choice, val, pos, n;
    struct Node* h2 = NULL;
    while (1) {
        printf("\n1.Create\n2.Insert\n3.Delete\n4.Reverse\n5.Merge\n6.Middle\n7.Sort\n8.Detect Loop\n9.Sum\n10.Odd-Even Print\n11.Display\n12.Exit\nEnter choice: ");
        scanf("%d", &choice);
        switch (choice) {
            case 1: printf("Enter number of nodes: "); scanf("%d", &n); create(n); break;
            case 2: printf("Enter position and value: "); scanf("%d%d", &pos, &val); insert(pos, val); break;
            case 3: printf("Enter position: "); scanf("%d", &pos); deleteNode(pos); break;
            case 4: reverse(); break;
            case 5: {
                int m, v;
                printf("Enter number of nodes in 2nd list: "); scanf("%d", &m);
                for (int i = 0; i < m; i++) {
                    printf("Enter data: "); scanf("%d", &v);
                    struct Node* newNode = createNode(v);
                    if (h2 == NULL) h2 = newNode;
                    else {
                        struct Node* t = h2;
                        while (t->next) t = t->next;
                        t->next = newNode;
                        newNode->prev = t;
                    }
                }
                merge(h2);
                break;
            }
            case 6: middle(); break;
            case 7: sort(); break;
            case 8: if (detectLoop()) printf("Loop detected\n"); else printf("No loop\n"); break;
            case 9: sumData(); break;
            case 10: printOddEven(); break;
            case 11: display(); break;
            case 12: exit(0);
            default: printf("Invalid choice\n");
        }
    }
    return 0;
}
