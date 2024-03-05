#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

// Structure representing a tree node
struct treeNode
{
    int data;               // Value of the node
    bool hasValue;          // Indicates if the node has a value
    struct treeNode* parentNode; // Pointer to the parent node
    struct treeNode* leftChild;  // Pointer to the left child node
    struct treeNode* rightChild; // Pointer to the right child node
};

// Function to set the root of the tree
void setRoot(struct treeNode** tree, int data)
{
    (*tree)->data = data;
    (*tree)->hasValue = true;
    (*tree)->parentNode = NULL;
    
    // Initialize child nodes with no values and link them to the parent
    (*tree)->rightChild = malloc(sizeof(struct treeNode));
    (*tree)->rightChild->hasValue = false;
    (*tree)->rightChild->rightChild = NULL;
    (*tree)->rightChild->leftChild = NULL;
    (*tree)->rightChild->parentNode = (*tree);

    (*tree)->leftChild = malloc(sizeof(struct treeNode));
    (*tree)->leftChild->hasValue = false;
    (*tree)->leftChild->rightChild = NULL;
    (*tree)->leftChild->leftChild = NULL;
    (*tree)->leftChild->parentNode = (*tree);
}

// Function to add a node to the tree
void addNode(struct treeNode** tree, int data)
{
    if (!(*tree)->hasValue)
    {
        // If the node has no value, set its value and initialize child nodes
        (*tree)->data = data;
        (*tree)->hasValue = true;
        (*tree)->rightChild = malloc(sizeof(struct treeNode));
        (*tree)->rightChild->hasValue = false;
        (*tree)->rightChild->rightChild = NULL;
        (*tree)->rightChild->leftChild = NULL;
        (*tree)->rightChild->parentNode = (*tree);

        (*tree)->leftChild = malloc(sizeof(struct treeNode));
        (*tree)->leftChild->hasValue = false;
        (*tree)->leftChild->rightChild = NULL;
        (*tree)->leftChild->leftChild = NULL;
        (*tree)->leftChild->parentNode = (*tree);
    } else
    {
        // If the node has a value, recursively add the node to the left or right child
        if ((*tree)->data > data)
            addNode(&(*tree)->leftChild, data);
        else
            addNode(&(*tree)->rightChild, data);
    }
}

void addStructNode(struct treeNode** tree, struct treeNode** node)
{
    if (!(*tree)->hasValue)
    {
        (*node)->parentNode = (*tree)->parentNode;
        (*tree) = *node;
    } else
    {
        // If the node has a value, recursively add the node to the left or right child
        if ((*tree)->data > (*node)->data)
            addStructNode(&(*tree)->leftChild, node);
        else
            addStructNode(&(*tree)->rightChild, node);
    }
}

// Function to check if a value is in the tree
bool isValueInTree(struct treeNode** tree, int value)
{
    bool isIn = false;
    if ((*tree)->data == value)
        return true;
    else
    {
        // If the value is greater, search in the right child; otherwise, search in the left child
        if ((*tree)->data < value && (*tree)->rightChild->hasValue)
            isIn = isValueInTree(&(*tree)->rightChild, value);
        else if ((*tree)->leftChild->hasValue)
            isIn = isValueInTree(&(*tree)->leftChild, value);
    }
    return isIn;
}

// Function to delete a node from the tree
void deleteNode(struct treeNode** tree, int value)
{
    if ((*tree)->hasValue)
    {
        if ((*tree)->data == value)
        {
            if(!(*tree)->rightChild->hasValue && !(*tree)->leftChild->hasValue)
            {
                if((*tree)->parentNode->data < value)
                    (*tree)->parentNode->rightChild->hasValue = false;
                else
                    (*tree)->parentNode->leftChild->hasValue = false;
            } else if((*tree)->rightChild->hasValue && !(*tree)->leftChild->hasValue)
            {
                if((*tree)->parentNode->data < (*tree)->rightChild->data)
                {
                    (*tree)->parentNode->rightChild = (*tree)->rightChild;
                    (*tree)->parentNode->rightChild->parentNode = (*tree)->parentNode;
                }else {
                    (*tree)->parentNode->leftChild = (*tree)->rightChild;
                    (*tree)->parentNode->leftChild->parentNode = (*tree)->parentNode;
                }
            } else if(!(*tree)->rightChild->hasValue && (*tree)->leftChild->hasValue)
            {
                if((*tree)->parentNode->data < (*tree)->leftChild->data)
                {
                    (*tree)->parentNode->rightChild = (*tree)->leftChild;
                    (*tree)->parentNode->rightChild->parentNode = (*tree)->parentNode;
                }else {
                    (*tree)->parentNode->leftChild = (*tree)->leftChild;
                    (*tree)->parentNode->leftChild->parentNode = (*tree)->parentNode;
                }
            } else 
            {
                if((*tree)->parentNode->data < (*tree)->data)
                {
                    struct treeNode** tempNode = &(*tree)->rightChild;
                    (*tree)->leftChild->parentNode = (*tree)->parentNode;
                    (*tree)->parentNode->rightChild = (*tree)->leftChild;
                    addStructNode(&(*tree), tempNode);
                } else 
                {
                    struct treeNode** tempNode = &(*tree)->rightChild;
                    (*tree)->leftChild->parentNode = (*tree)->parentNode;
                    (*tree)->parentNode->leftChild = (*tree)->leftChild;
                    addStructNode(&(*tree), tempNode);
                }
            }
        } else
        {
            // If the value is greater, recursively delete in the right child; otherwise, delete in the left child
            if ((*tree)->data < value)
                deleteNode(&(*tree)->rightChild, value);
            else
                deleteNode(&(*tree)->leftChild, value);
        }
    }
}

// Function to print the tree
void printTree(struct treeNode** tree, int level)
{
    if (!(*tree)->hasValue)
        return;
    // Recursively print the right child, current node, and then the left child
    printTree(&(*tree)->rightChild, level + 1);
    for (int i = 0; i < level; i++)
        printf("  "); 
    printf("%d\n", (*tree)->data);

    printTree(&(*tree)->leftChild, level + 1);
}

void printTreePosOrder(struct treeNode** tree)
{
    if (!(*tree)->hasValue) 
        return;
    printTreePosOrder(&(*tree)->leftChild);
    printTreePosOrder(&(*tree)->rightChild);
    if((*tree)->parentNode != NULL)
        printf("%d, ", (*tree)->data);
    else
        printf("Root: %d ", (*tree)->data);
}

void printTreePreeOrder(struct treeNode** tree)
{
    if (!(*tree)->hasValue) 
        return;
    printTreePreeOrder(&(*tree)->rightChild);
    printTreePreeOrder(&(*tree)->leftChild);
    if((*tree)->parentNode != NULL)
        printf("%d, ", (*tree)->data);
    else
        printf("Root: %d ", (*tree)->data);
}

// Function to get the size of the tree
int getTreeSize(struct treeNode** tree)
{
    int size = 0;
    if ((*tree)->hasValue)
    {
        size++;
        if ((*tree)->rightChild->hasValue)
            size += getTreeSize(&(*tree)->rightChild);
        if ((*tree)->leftChild->hasValue)
            size += getTreeSize(&(*tree)->leftChild);
    }
    return size;
}

// Function to free the memory used by the tree
void freeTreeMemory(struct treeNode** tree)
{
    if ((*tree)->hasValue)
    {
        // Recursively free the memory of the left and right child nodes, and then free the current node
        freeTreeMemory(&(*tree)->leftChild);
        freeTreeMemory(&(*tree)->rightChild);
        free((*tree));
    }
}

// Main function
int main()
{
    struct treeNode* tree = malloc(sizeof(struct treeNode));
    bool hasRoot = false;
    bool run = true;
    while (run)
    {
        int option;
        printf("Select a number from the menu:\n");
        printf("1) Add Node\n");
        printf("2) Is the value in the tree?\n");
        printf("3) Delete Value\n");
        printf("4) Get Tree Size\n");
        printf("5) Print Tree\n");
        printf("6) Print in PosOrder\n");
        printf("7) Print in PreeOrder\n");
        printf("8) Exit\n");
        printf("Enter the option:\n");
        scanf("%d", &option);
        int node;
        printf("\n");
        switch (option)
        {
        case 1:
            printf("Enter the number to add:\n");
            scanf("%d", &node);
            if (!hasRoot)
            {
                setRoot(&tree, node);
                hasRoot = true;
            }
            else
                addNode(&tree, node);
            printf("Added Successfully\n");
            break;
        case 2:
            printf("Enter the number to search:\n");
            scanf("%d", &node);
            if (isValueInTree(&tree, node))
                printf("The value is in the tree\n");
            else
                printf("The value is not in the tree\n");
            break;
        case 3:
            printf("Enter the number to delete:\n");
            scanf("%d", &node);
            deleteNode(&tree, node);
            printf("Value Deleted\n");
            break;
        case 4:
            printf("Tree Size: %d\n", getTreeSize(&tree));
            break;
        case 5:
            printf("Tree:\n");
            printTree(&tree, 0);
            break;
        case 6:
            printf("PosOrder:\n");
            printTreePosOrder(&tree);
            printf("\n");
        break;
        case 7:
            printf("PreeOrder:\n");
            printTreePreeOrder(&tree);
            printf("\n");
        break;
        case 8:
            run = false;
            printf("Goodbye\n");
            break;
        default:
            printf("Invalid Option\n");
            break;
        }
        printf("\n\n");
    }
    freeTreeMemory(&tree);
    return 0;
}
