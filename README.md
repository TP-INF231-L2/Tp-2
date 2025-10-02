
#include <stdio.h>
#include <stdlib.h>

// ----------------- Exercice 1 -----------------
struct Node
{
	int data;
	struct Node *next;
};

struct Node *newNode(int data)
{
	struct Node *node = (struct Node *)malloc(sizeof(struct Node));
	node->data = data;
	node->next = NULL;
	return node;
}

void supprimerOccurrences(struct Node **head, int val)
{
	struct Node *temp = *head;
	struct Node *prev = NULL;

	while (temp != NULL && temp->data == val)
	{
		*head = temp->next;
		free(temp);
		temp = *head;
	}

	while (temp != NULL)
	{
		if (temp->data == val)
		{
			prev->next = temp->next;
			free(temp);
		}
		else
		{
			prev = temp;
		}
		temp = prev->next;
	}
}

void afficher(struct Node *head)
{
	while (head != NULL)
	{
		printf("%d -> ", head->data);
		head = head->next;
	}
	printf("NULL\n");
}

void exercice1()
{
	struct Node *head = newNode(1);
	head->next = newNode(2);
	head->next->next = newNode(3);
	head->next->next->next = newNode(2);
	head->next->next->next->next = newNode(4);

	printf("Liste avant suppression :\n");
	afficher(head);

	int x;
	printf("Entrez l'élément à supprimer : ");
	scanf("%d", &x);

	supprimerOccurrences(&head, x);

	printf("Liste après suppression :\n");
	afficher(head);
}

// ----------------- Exercice 2 -----------------

void insertSorted(struct Node **head, int value)
{
	struct Node *newNode1 = newNode(value);
	if (*head == NULL || value < (*head)->data)
	{
		newNode1->next = *head;
		*head = newNode1;
	}
	else
	{
		struct Node *current = *head;
		while (current->next != NULL && current->next->data < value)
		{
			current = current->next;
		}
		newNode1->next = current->next;
		current->next = newNode1;
	}
}

void exercice2()
{
	struct Node *head = NULL;
	insertSorted(&head, 10);
	insertSorted(&head, 5);
	insertSorted(&head, 20);
	insertSorted(&head, 15);

	printf("Liste triée : ");
	afficher(head);
}

// ----------------- Exercice 3 -----------------

struct DNode
{
	int data;
	struct DNode *prev;
	struct DNode *next;
};

struct DNode *newDNode(int value)
{
	struct DNode *node = (struct DNode *)malloc(sizeof(struct DNode));
	node->data = value;
	node->prev = NULL;
	node->next = NULL;
	return node;
}

void insertSortedD(struct DNode **head, int value)
{
	struct DNode *newNode = newDNode(value);
	if (*head == NULL || value < (*head)->data)
	{
		newNode->next = *head;
		if (*head != NULL)
			(*head)->prev = newNode;
		*head = newNode;
	}
	else
	{
		struct DNode *current = *head;
		while (current->next != NULL && current->next->data < value)
		{
			current = current->next;
		}
		newNode->next = current->next;
		if (current->next != NULL)
			current->next->prev = newNode;
		newNode->prev = current;
		current->next = newNode;
	}
}

void afficherD(struct DNode *head)
{
	struct DNode *current = head;
	while (current != NULL)
	{
		printf("%d <-> ", current->data);
		current = current->next;
	}
	printf("NULL\n");
}

void exercice3()
{
	struct DNode *head = NULL;
	insertSortedD(&head, 10);
	insertSortedD(&head, 5);
	insertSortedD(&head, 20);
	insertSortedD(&head, 15);

	printf("Liste doublement triée : ");
	afficherD(head);
}

// ----------------- Exercice 4 -----------------

struct CNode
{
	int data;
	struct CNode *next;
};

struct CNode *newCNode(int value)
{
	struct CNode *node = (struct CNode *)malloc(sizeof(struct CNode));
	node->data = value;
	node->next = node;
	return node;
}

void insertHeadC(struct CNode **head, int value)
{
	struct CNode *newNode = newCNode(value);
	if (*head == NULL)
	{
		*head = newNode;
	}
	else
	{
		struct CNode *temp = *head;
		while (temp->next != *head)
			temp = temp->next;
		temp->next = newNode;
		newNode->next = *head;
		*head = newNode;
	}
}

void insertTailC(struct CNode **head, int value)
{
	struct CNode *newNode = newCNode(value);
	if (*head == NULL)
	{
		*head = newNode;
	}
	else
	{
		struct CNode *temp = *head;
		while (temp->next != *head)
			temp = temp->next;
		temp->next = newNode;
		newNode->next = *head;
	}
}

void afficherC(struct CNode *head)
{
	if (head == NULL)
		return;
	struct CNode *temp = head;
	do
	{
		printf("%d -> ", temp->data);
		temp = temp->next;
	} while (temp != head);
	printf("(retour au début)\n");
}

void exercice4()
{
	struct CNode *head = NULL;
	insertHeadC(&head, 10);
	insertHeadC(&head, 5);
	insertTailC(&head, 20);
	insertTailC(&head, 25);

	printf("Liste circulaire simplement chaînée : ");
	afficherC(head);
}

// ----------------- Exercice 5 -----------------

struct DCNode
{
	int data;
	struct DCNode *prev;
	struct DCNode *next;
};

struct DCNode *newDCNode(int value)
{
	struct DCNode *node = (struct DCNode *)malloc(sizeof(struct DCNode));
	node->data = value;
	node->prev = node;
	node->next = node;
	return node;
}

void insertHeadDC(struct DCNode **head, int value)
{
	struct DCNode *newNode = newDCNode(value);
	if (*head == NULL)
	{
		*head = newNode;
	}
	else
	{
		struct DCNode *tail = (*head)->prev;
		newNode->next = *head;
		newNode->prev = tail;
		tail->next = newNode;
		(*head)->prev = newNode;
		*head = newNode;
	}
}

void insertTailDC(struct DCNode **head, int value)
{
	struct DCNode *newNode = newDCNode(value);
	if (*head == NULL)
	{
		*head = newNode;
	}
	else
	{
		struct DCNode *tail = (*head)->prev;
		newNode->next = *head;
		newNode->prev = tail;
		tail->next = newNode;
		(*head)->prev = newNode;
	}
}

void afficherDC(struct DCNode *head)
{
	if (head == NULL)
		return;
	struct DCNode *temp = head;
	do
	{
		printf("%d <-> ", temp->data);
		temp = temp->next;
	} while (temp != head);
	printf("(retour au début)\n");
}

void exercice5()
{
	struct DCNode *head = NULL;
	insertHeadDC(&head, 10);
	insertHeadDC(&head, 5);
	insertTailDC(&head, 20);
	insertTailDC(&head, 25);

	printf("Liste doublement circulaire : ");
	afficherDC(head);
}
