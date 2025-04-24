# EDSA-project
#Homework
#include <stdio.h> #include <stdlib.h> #include <string.h>



// Queue Implementation struct QueueNode {

char item[20];

struct QueueNode* next;

};





struct Queue {

struct QueueNode* front; struct QueueNode* rear;

};





void enqueue(struct Queue* q, const char* item) {

struct QueueNode* newNode = (struct QueueNode*)malloc(sizeof(struct QueueNode));

strcpy(newNode->item, item); newNode->next = NULL;

if (q->rear == NULL) {

q->front = q->rear = newNode;

} else {

q->rear->next = newNode; q->rear = newNode;

}

}

 

// Stack Implementation struct StackNode {

char item[20];

struct StackNode* next;

};





void push(struct StackNode** stack, const char* item) {

struct StackNode* newNode = (struct StackNode*)malloc(sizeof(struct StackNode));

strcpy(newNode->item, item); newNode->next = *stack;

*stack = newNode;

}





void dequeueToStack(struct Queue* q, struct StackNode** stack) { while (q->front != NULL) {

struct QueueNode* temp = q->front; push(stack, temp->item);

q->front = q->front->next; free(temp);

}

q->rear = NULL;

}





void displayDispatch(struct StackNode** stack) { while (*stack) {

printf("Dispatched: %s\n", (*stack)->item); struct StackNode* temp = *stack;

 

*stack = (*stack)->next; free(temp);

}

}





// Flight Log struct FlightLog {

char** entries; int capacity; int count;

};





void initFlightLog(struct FlightLog* log, int capacity) { log->entries = malloc(sizeof(char*) * capacity);

for (int i = 0; i < capacity; i++) log->entries[i] = malloc(20);

log->capacity = capacity; log->count = 0;

}





void logDelivery(struct FlightLog* log, const char* delivery) { if (log->count < log->capacity) {

strcpy(log->entries[log->count++], delivery);

} else {

for (int i = 1; i < log->capacity; i++) strcpy(log->entries[i - 1], log->entries[i]);

strcpy(log->entries[log->capacity - 1], delivery);

}

}

 

void displayFlightLog(struct FlightLog* log) { printf("\nFlight Log:\n");

for (int i = 0; i < log->count; i++) printf("%s\n", log->entries[i]);

}





void freeFlightLog(struct FlightLog* log) { for (int i = 0; i < log->capacity; i++)

free(log->entries[i]); free(log->entries);

}





// Overloaded Drones (Singly Linked List) struct SinglyNode {

char drone_id[20]; struct SinglyNode* next;

};



void addSinglyNode(struct SinglyNode** head, const char* drone_id) { struct SinglyNode* newNode = (struct SinglyNode*)malloc(sizeof(struct

SinglyNode));

strcpy(newNode->drone_id, drone_id); newNode->next = *head;

*head = newNode;

}





// Serviced Drones (Doubly Linked List) struct DoublyNode {

 

char drone_id[20];

struct DoublyNode* next; struct DoublyNode* prev;

};





void moveToDoubly(struct SinglyNode** singlyHead, struct DoublyNode** doublyHead) {

if (*singlyHead == NULL) return;

struct SinglyNode* temp = *singlyHead;

*singlyHead = temp->next;





struct DoublyNode* newNode = (struct DoublyNode*)malloc(sizeof(struct DoublyNode));

strcpy(newNode->drone_id, temp->drone_id); newNode->next = *doublyHead;

newNode->prev = NULL; if (*doublyHead != NULL)

(*doublyHead)->prev = newNode;

*doublyHead = newNode;





free(temp);

}





void traverseDoubly(struct DoublyNode* head) { struct DoublyNode* last = NULL; printf("\nForward: ");

while (head) {

printf("%s ", head->drone_id); last = head;

head = head->next;

 

}

printf("\nBackward: "); while (last) {

printf("%s ", last->drone_id); last = last->prev;

}

printf("\n");

}





// Emergency Reroute (Circular Linked List) struct CircularNode {

char drone_id[20];

struct CircularNode* next;

};



void addCircularNode(struct CircularNode** head, const char* drone_id) { struct CircularNode* newNode = (struct CircularNode*)malloc(sizeof(struct

CircularNode));

strcpy(newNode->drone_id, drone_id); if (*head == NULL) {

newNode->next = newNode;

*head = newNode;

} else {

struct CircularNode* temp = *head; while (temp->next != *head)

temp = temp->next; temp->next = newNode; newNode->next = *head;

}

 

}





void traverseCircular(struct CircularNode* head) { if (head == NULL) return;

struct CircularNode* temp = head; int count = 0;

printf("\nEmergency Reroute: "); do {

printf("%s ", temp->drone_id); temp = temp->next;

count++;

} while (temp != head && count < 4); printf("\n");

}





// Main Function int main() {

struct Queue deliveryQueue = { NULL, NULL }; struct StackNode* dispatchStack = NULL; struct FlightLog flightLog;

struct SinglyNode* overloaded = NULL; struct DoublyNode* serviced = NULL; struct CircularNode* reroute = NULL;



initFlightLog(&flightLog, 6);





int choice;

char drone_id[20]; while (1) {

 

printf("\n1. Queue & Dispatch Deliveries\n2. Log Deliveries\n3. Add Overloaded Drone\n4. Service Drone\n5. View Maintenance Log\n6. Add Emergency Reroute\n7. View Emergency Route\n0. Exit\nChoice: ");

scanf("%d", &choice);





switch (choice) { case 1:

enqueue(&deliveryQueue, "Food"); enqueue(&deliveryQueue, "Medicine"); enqueue(&deliveryQueue, "Fuel"); enqueue(&deliveryQueue, "Spare Parts"); enqueue(&deliveryQueue, "Tools"); enqueue(&deliveryQueue, "Water"); dequeueToStack(&deliveryQueue, &dispatchStack); displayDispatch(&dispatchStack);

break; case 2:

for (int i = 1; i <= 8; i++) { sprintf(drone_id, "Del%d", i); logDelivery(&flightLog, drone_id);

}

displayFlightLog(&flightLog); break;

case 3:

printf("Enter Drone ID: "); scanf("%s", drone_id);

addSinglyNode(&overloaded, drone_id); break;

case 4:

moveToDoubly(&overloaded, &serviced);

 

break; case 5:

traverseDoubly(serviced); break;

case 6:

printf("Enter Drone ID for Emergency Reroute: "); scanf("%s", drone_id); addCircularNode(&reroute, drone_id);

break; case 7:

traverseCircular(reroute); break;

case 0:

freeFlightLog(&flightLog); printf("Exiting...\n"); exit(0);

default:

printf("Invalid choice.\n");

}

}

return 0;

}

