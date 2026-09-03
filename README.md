code
# Pabitra-1.github.io





#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    int height;
    struct Node *left;
    struct Node *right;
};

/* Return maximum of two numbers */
int max(int a, int b) {
    return (a > b) ? a : b;
}

/* Return height of a node */
int height(struct Node *node) {
    if (node == NULL)
        return 0;

    return node->height;
}

/* Create a new node */
struct Node* createNode(int data) {
    struct Node *newNode =
        (struct Node*)malloc(sizeof(struct Node));

    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    newNode->height = 1;

    return newNode;
}

/* Calculate balance factor */
int getBalance(struct Node *node) {
    if (node == NULL)
        return 0;

    return height(node->left) - height(node->right);
}

/* Right Rotation - LL Case */
struct Node* rightRotate(struct Node *y) {
    struct Node *x = y->left;
    struct Node *T2 = x->right;

    x->right = y;
    y->left = T2;

    y->height = 1 + max(height(y->left), height(y->right));
    x->height = 1 + max(height(x->left), height(x->right));

    return x;
}

/* Left Rotation - RR Case */
struct Node* leftRotate(struct Node *x) {
    struct Node *y = x->right;
    struct Node *T2 = y->left;

    y->left = x;
    x->right = T2;

    x->height = 1 + max(height(x->left), height(x->right));
    y->height = 1 + max(height(y->left), height(y->right));

    return y;
}

/* Insert a node into AVL tree */
struct Node* insert(struct Node *node, int data) {

    /* Normal BST insertion */
    if (node == NULL)
        return createNode(data);

    if (data < node->data)
        node->left = insert(node->left, data);
    else if (data > node->data)
        node->right = insert(node->right, data);
    else
        return node;   // Duplicate values are ignored

    /* Update height */
    node->height =
        1 + max(height(node->left), height(node->right));

    /* Calculate balance factor */
    int balance = getBalance(node);

    /* LL Case */
    if (balance > 1 && data < node->left->data)
        return rightRotate(node);

    /* RR Case */
    if (balance < -1 && data > node->right->data)
        return leftRotate(node);

    /* LR Case */
    if (balance > 1 && data > node->left->data) {
        node->left = leftRotate(node->left);
        return rightRotate(node);
    }

    /* RL Case */
    if (balance < -1 && data < node->right->data) {
        node->right = rightRotate(node->right);
        return leftRotate(node);
    }

    return node;
}

/* Inorder traversal */
void inorder(struct Node *root) {
    if (root != NULL) {
        inorder(root->left);
        printf("%d ", root->data);
        inorder(root->right);
    }
}

/* Display height and balance factor of every node */
void displayDetails(struct Node *root) {
    if (root != NULL) {
        displayDetails(root->left);

        printf("Node = %d, Height = %d, Balance Factor = %d\n",
               root->data,
               root->height,
               getBalance(root));

        displayDetails(root->right);
    }
}

int main() {
    struct Node *root = NULL;
    int n, data;

    printf("Enter number of elements: ");
    scanf("%d", &n);

    printf("Enter %d integers:\n", n);

    for (int i = 0; i < n; i++) {
        scanf("%d", &data);
        root = insert(root, data);
    }

    printf("\nFinal AVL Tree - Inorder Traversal:\n");
    inorder(root);

    printf("\n\nHeight and Balance Factor of each node:\n");
    displayDetails(root);

    return 0;
}



## 2nd code

#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    int height;
    struct Node *left;
    struct Node *right;
};

int max(int a, int b) {
    return (a > b) ? a : b;
}

int height(struct Node *node) {
    if (node == NULL)
        return 0;

    return node->height;
}

struct Node* createNode(int data) {
    struct Node *newNode =
        (struct Node*)malloc(sizeof(struct Node));

    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    newNode->height = 1;

    return newNode;
}

int getBalance(struct Node *node) {
    if (node == NULL)
        return 0;

    return height(node->left) - height(node->right);
}

/* Right Rotation */
struct Node* rightRotate(struct Node *y) {
    struct Node *x = y->left;
    struct Node *T2 = x->right;

    x->right = y;
    y->left = T2;

    y->height =
        1 + max(height(y->left), height(y->right));

    x->height =
        1 + max(height(x->left), height(x->right));

    return x;
}

/* Left Rotation */
struct Node* leftRotate(struct Node *x) {
    struct Node *y = x->right;
    struct Node *T2 = y->left;

    y->left = x;
    x->right = T2;

    x->height =
        1 + max(height(x->left), height(x->right));

    y->height =
        1 + max(height(y->left), height(y->right));

    return y;
}

/* Insert */
struct Node* insert(struct Node *node, int data) {

    if (node == NULL)
        return createNode(data);

    if (data < node->data)
        node->left = insert(node->left, data);
    else if (data > node->data)
        node->right = insert(node->right, data);
    else
        return node;

    node->height =
        1 + max(height(node->left), height(node->right));

    int balance = getBalance(node);

    /* LL */
    if (balance > 1 && data < node->left->data)
        return rightRotate(node);

    /* RR */
    if (balance < -1 && data > node->right->data)
        return leftRotate(node);

    /* LR */
    if (balance > 1 && data > node->left->data) {
        node->left = leftRotate(node->left);
        return rightRotate(node);
    }

    /* RL */
    if (balance < -1 && data < node->right->data) {
        node->right = rightRotate(node->right);
        return leftRotate(node);
    }

    return node;
}

/* Search an element */
struct Node* search(struct Node *root, int key) {

    if (root == NULL)
        return NULL;

    if (key == root->data)
        return root;

    if (key < root->data)
        return search(root->left, key);

    return search(root->right, key);
}

/* Inorder traversal */
void inorder(struct Node *root) {
    if (root != NULL) {
        inorder(root->left);
        printf("%d ", root->data);
        inorder(root->right);
    }
}

int main() {
    struct Node *root = NULL;
    int n, data, key;

    printf("Enter number of elements: ");
    scanf("%d", &n);

    printf("Enter %d integers:\n", n);

    for (int i = 0; i < n; i++) {
        scanf("%d", &data);
        root = insert(root, data);
    }

    printf("\nAVL Tree (Inorder): ");
    inorder(root);

    printf("\n\nEnter element to search: ");
    scanf("%d", &key);

    if (search(root, key) != NULL)
        printf("%d is present in the AVL tree.\n", key);
    else
        printf("%d is not present in the AVL tree.\n", key);

    return 0;
}


## 3rd code
#include <stdio.h>
#include <stdlib.h>

struct Node {
    int data;
    int height;
    struct Node *left;
    struct Node *right;
};

int max(int a, int b) {
    return (a > b) ? a : b;
}

/* Get height */
int height(struct Node *node) {
    if (node == NULL)
        return 0;

    return node->height;
}

/* Create new node */
struct Node* createNode(int data) {
    struct Node *newNode =
        (struct Node*)malloc(sizeof(struct Node));

    newNode->data = data;
    newNode->left = NULL;
    newNode->right = NULL;
    newNode->height = 1;

    return newNode;
}

/* Get balance factor */
int getBalance(struct Node *node) {
    if (node == NULL)
        return 0;

    return height(node->left) - height(node->right);
}

/* Right Rotation */
struct Node* rightRotate(struct Node *y) {

    struct Node *x = y->left;
    struct Node *T2 = x->right;

    x->right = y;
    y->left = T2;

    y->height =
        1 + max(height(y->left), height(y->right));

    x->height =
        1 + max(height(x->left), height(x->right));

    return x;
}

/* Left Rotation */
struct Node* leftRotate(struct Node *x) {

    struct Node *y = x->right;
    struct Node *T2 = y->left;

    y->left = x;
    x->right = T2;

    x->height =
        1 + max(height(x->left), height(x->right));

    y->height =
        1 + max(height(y->left), height(y->right));

    return y;
}

/* Insert node */
struct Node* insert(struct Node *node, int data) {

    if (node == NULL)
        return createNode(data);

    if (data < node->data)
        node->left = insert(node->left, data);
    else if (data > node->data)
        node->right = insert(node->right, data);
    else
        return node;

    node->height =
        1 + max(height(node->left), height(node->right));

    int balance = getBalance(node);

    /* LL Case */
    if (balance > 1 && data < node->left->data)
        return rightRotate(node);

    /* RR Case */
    if (balance < -1 && data > node->right->data)
        return leftRotate(node);

    /* LR Case */
    if (balance > 1 && data > node->left->data) {
        node->left = leftRotate(node->left);
        return rightRotate(node);
    }

    /* RL Case */
    if (balance < -1 && data < node->right->data) {
        node->right = rightRotate(node->right);
        return leftRotate(node);
    }

    return node;
}

/* Find node with minimum value */
struct Node* minValueNode(struct Node *node) {

    struct Node *current = node;

    while (current->left != NULL)
        current = current->left;

    return current;
}

/* Delete node */
struct Node* deleteNode(struct Node *root, int key) {

    /* Normal BST deletion */
    if (root == NULL)
        return root;

    if (key < root->data) {
        root->left = deleteNode(root->left, key);
    }
    else if (key > root->data) {
        root->right = deleteNode(root->right, key);
    }
    else {

        /* Node has no child or only one child */
        if (root->left == NULL || root->right == NULL) {

            struct Node *temp;

            if (root->left != NULL)
                temp = root->left;
            else
                temp = root->right;

            /* No child */
            if (temp == NULL) {
                temp = root;
                root = NULL;
            }
            /* One child */
            else {
                *root = *temp;
            }

            free(temp);
        }

        /* Node has two children */
        else {

            struct Node *temp =
                minValueNode(root->right);

            root->data = temp->data;

            root->right =
                deleteNode(root->right, temp->data);
        }
    }

    /* Tree became empty */
    if (root == NULL)
        return root;

    /* Update height */
    root->height =
        1 + max(height(root->left), height(root->right));

    /* Calculate balance factor */
    int balance = getBalance(root);

    /* LL Case */
    if (balance > 1 &&
        getBalance(root->left) >= 0)
        return rightRotate(root);

    /* LR Case */
    if (balance > 1 &&
        getBalance(root->left) < 0) {

        root->left =
            leftRotate(root->left);

        return rightRotate(root);
    }

    /* RR Case */
    if (balance < -1 &&
        getBalance(root->right) <= 0)
        return leftRotate(root);

    /* RL Case */
    if (balance < -1 &&
        getBalance(root->right) > 0) {

        root->right =
            rightRotate(root->right);

        return leftRotate(root);
    }

    return root;
}

/* Inorder traversal */
void inorder(struct Node *root) {

    if (root != NULL) {
        inorder(root->left);
        printf("%d ", root->data);
        inorder(root->right);
    }
}

int main() {

    struct Node *root = NULL;
    int n, data, key;

    printf("Enter number of elements: ");
    scanf("%d", &n);

    printf("Enter %d integers:\n", n);

    for (int i = 0; i < n; i++) {
        scanf("%d", &data);
        root = insert(root, data);
    }

    printf("\nAVL Tree before deletion:\n");
    inorder(root);

    printf("\n\nEnter node to delete: ");
    scanf("%d", &key);

    root = deleteNode(root, key);

    printf("\nAVL Tree after deletion:\n");
    inorder(root);

    return 0;
}
