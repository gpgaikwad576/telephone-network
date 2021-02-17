# telephone-network

#include<stdio.h>
#include<stdlib.h>
#include<math.h>

//Node for Vertex
typedef struct Node {
    int vertex;
    char visitStatus;
    float fixedWeight;
    int cameFrom;
    struct Node * next;
    struct Node * prev;
}Vertex;
Vertex * head = NULL;

//Node for Edge
typedef struct node {
    int source;
    int destination;
    int weight;
    struct node * next;
    struct node * prev;
}Edge;
Edge * start = NULL;

//Node for heap
typedef struct heap {
    int Weight;
    int V;
    int D;
    int visited;
    struct heap * left;
    struct heap * right;
    struct heap * parent;
}Heap;

//Creation Vertex
Vertex * createVertex(int vertex){
    Vertex * new = (Vertex *)malloc (sizeof(Vertex));
    new->vertex = vertex;
    new->next = NULL;
    new->prev = NULL;
    new->fixedWeight = INFINITY;
    new->visitStatus = 'N';
    if (head ==  NULL)
        head = new;
    else{
        Vertex * tmp = head;
        int present = 0;
        while (tmp->next != NULL)
        {
            if (tmp->vertex == vertex)
                present = 1;
            tmp = tmp->next;
        }
        if (present == 1)
        {
            printf("The index is already present !\n");
        }
        else if (present != 1)
        {
            tmp->next = new;
            new->prev = tmp;
        }
    }
}

int cnt;
//Display of Vertex
void printVertex(){
    cnt = 0;
    printf("The Regenerative Repeater : ");
    Vertex * temp = head;
    while (temp!= NULL)
    {
        printf("%d  ",temp->vertex);
        temp = temp->next;
        cnt++;
    }
    printf("\n");
}

//Display of Edge
void printEdge(){
    printf("The Current link in Repeater : ");
    Edge * temp = start;
    while (temp!= NULL)
    {
        printf("[%d,%d,%d]  ",temp->source,temp->destination,temp->weight);
        temp = temp->next;
    }
    printf("\n");
}

//Deletion of Edge
Edge * deleteEdge(int source, int destination){
    Edge * temp = start ;
    if (temp->source == source && temp->destination == destination)
    {
        start = temp->next;
        return temp;
    }
    while (temp != NULL)
    {
        if (temp->source == source && temp->destination == destination)
        {
            temp->prev->next = temp->next;
            temp->next->prev = temp->prev;
            return temp;
        }
        temp = temp->next;
    }
    printf("The link between Regenerative repeater is not found.\n");
    return NULL;
}

//Creation and Updation of Edge
Edge * addEdge(int source, int destination, int weight){
    Edge * new = (Edge * ) malloc (sizeof(Edge));
    new->source = source;
    new->destination = destination;
    new->weight = weight;
    new->next = NULL;
    new->prev = NULL;
    if (start == NULL)
        start = new;
    else{
        Edge * temp = start;
        while (temp->next != NULL)
        {
            if (temp->source == source && temp->destination == destination && temp->weight == weight)
            {
                printf(" *** The link is already present ! *** \n");
                return temp;
            }
            else if (temp->source == source && temp->destination == destination)
            {
               deleteEdge(source,destination);
            }
            temp = temp->next;
        }
        temp->next = new;
        new->prev = temp;
    }
}

//Presence of Vertex in Graph
int checkValidity(int source, int destination){
    Vertex * temp = head;
    int cnt = 0, found;
    while (temp != NULL)
    {
        if (temp->vertex == source){
            cnt++;
            found = source;
        }
        else if (temp->vertex == destination){
            cnt++;
            found = destination;
        }
        temp = temp->next;
    }
    if (cnt == 2)
        return 1;
    else if (found == destination)
        return 2;
    else if (found == source)
        return 3;
    else
        return 4;
}

//Creation Edge for new Vertex *Compulsory
void enterEdge(int vertex){
    printf("\nIt is compulsory to have atleast one indegree and one outdegree link.\n");
    int linked , weight;
    int cnt[2] = {0, 0};
    char resp;
    while (1)
    {
        printVertex();
        printf("\nPlease dont link with %d as it will be the vertex itself.\n",vertex);
        printf("\nDo you want to choose the repeator %d as (Indegree I) and (Outdegree O) : ",vertex);
        scanf(" %c",&resp);
        if (resp == 'I' || resp == 'i')
        {
            printf("The destination is %d repeator.\n",vertex);
            printf("Source : ");
            scanf("%d",&linked);
            printf("Weight : ");
            scanf("%d",&weight);
            if (vertex == linked){
                printf("Source and Destination must not be same.\n");
                continue;
            }
            else if (checkValidity(vertex, linked) == 1){
                addEdge(linked,vertex,weight);
                cnt[0]++;
            }
        }
        else if (resp == 'O' || resp == 'o')
        {
            printf("The source is %d repeator.\n",vertex);
            printf("Destination : ");
            scanf("%d",&linked);
            printf("Weight : ");
            scanf("%d",&weight);
            if (vertex == linked){
                printf("Source and Destination must not be same.\n");
                continue;
            }
            else if (checkValidity(vertex, linked) == 1){
                addEdge(vertex,linked,weight);
                cnt[1]++;
            }
        }
        printf("Do you want to add more link (Y : Yes and N : No ): ");
        scanf(" %c",&resp);
        if (resp == 'Y' || resp == 'y')
            continue;
        else{
            if (cnt[0] > 0 && cnt[1] > 0)
            {
                break;
            }
            else if (cnt[0] == 0)
            {
                printf("It is compulsory to have atleast one indegree and one outdegree link.\n");
                printf("\n *** Enter atleast one indegree link. ***\n");
            }
            else if (cnt[1] == 0)
            {
                printf("It is compulsory to have atleast one indegree and one outdegree link.\n");
                printf("\n *** Enter atleast one outdegree link. ***\n");
            }
            else
            {
                printf("\n *** It is compulsory to have atleast one indegree and one outdegree link. ***\n");
            }
        }
    }
}

//Deletion of Vertex
Vertex * deleteVertex(int vertex){
    Vertex * temp = head;
    if (temp->vertex == vertex)
    {
        head = temp->next;
        return temp;
    }
    while (temp != NULL)
    {
        if (temp->vertex == vertex)
        {
            temp->prev->next = temp->next;
            temp->next->prev = temp->prev;
            return temp;
        }
        temp = temp->next;
    }
    printf("The Regenerative repeater is not yet installed.\n");
    return NULL;
}

//Count the nodes in heap
int count(Heap * p,int n)
{
   if (p == NULL || p->visited == n)
       return 0;
   else
   {
       int lcnt = count(p->left,n);
       int rcnt = count(p->right,n);
       return(lcnt+rcnt+1);
   }
}

//Swaping two nodes
void swap(Heap * node1,Heap * node2){
    int tempWeight = node1->Weight;
    int tempD = node1->D;
    node1->Weight = node2->Weight;
    node2->Weight = tempWeight;
    node1->D = node2->D;
    node2->D = tempD;
    return;
}

//Minimum Heap checking
void arrangeMIN(Heap * A){
    if (A->parent == NULL)
    {
        return;
    }
    else if (A->parent != NULL)
    {
        if (A->Weight > A->parent->Weight)
        {
            swap(A, A->parent);
            arrangeMIN(A->parent);
        }
        else
        {
            return;
        }
    }
}

//Checking the complete tree
int FullTree(int num){
    while (num != 0)
    {
        if (num == 1)
        {
            return 1;
        }
        else if (num % 2 == 0)
        {
            return 0;
        }
        num = num / 2;
    }
    return 0;
}

//Creating the heap
Heap * createHeap(Heap * A, Heap * new){
    if (A->left == NULL)
    {
        new->parent = A;
        A->left = new;
        // printf("HI left %d of %d.\n",new->index,root->index);
        arrangeMIN(new);
        // printf("The node to check for precidence is %d and %d.\n\n",new->index,new->parent->index);
        return A;

    }
    else if (A->right == NULL)
    {
        new->parent = A;
        A->right = new;
        // printf("HI right %d of %d.\n",new->index,root->index);
        arrangeMIN(new);
        // printf("The node to check for precidence is %d and %d.\n\n",new->index,new->parent->index);
        return A;
    }
    else
    {
        int k = count(A->left,1);
        int s = count(A->right,1);
        printf("Left count - %d Right count - %d .\n",k,s);
        if (k == s)
        {
            // printf("to  move to left of %d.\n",root->index);
            return createHeap(A->left,new);
        }
        else if (k%2 != 0 && FullTree(k))
        {
            // printf("to  move to right of %d.\n",root->index);
            return createHeap(A->right,new);
        }
        else
        {
            // printf("to  move to left of %d.\n",root->index);
            return createHeap(A->left,new);
        }
    }
}

//Adding to the heap
Heap * addToHeap(Heap ** A,int source, int destination, int weight){
    Heap * new = (Heap *) malloc(sizeof(Heap));
    new->V = source;
    new->Weight = weight;
    new->D = destination;
    new->visited = 0;
    new->left = NULL;
    new->right = NULL;
    new->parent = NULL;
    int insert;
    for (int i = 0; i < cnt; i++)
    {
        if (A[i] == NULL)
        {
            insert = i;
            break;
        }
        else if (A[i]->V == new->V)
        {
            createHeap(A[i], new);
            return A[i];
        }
    }
    A[insert] = (Heap *) malloc(sizeof(Heap));
    A[insert] = new;
}

//printing given level
void printGivenLevel(Heap * T, int level)
{
    if (T == NULL)
        return;
    if (level == 1)
        printf("[%d %d %d] ", T->Weight,T->D,T->visited);
    else if (level > 1)
    {
        printGivenLevel(T->left, level-1);
        printGivenLevel(T->right, level-1);
    }
}

//printing height
int height(Heap * T)
{
    if (T==NULL)
        return 0;
    else
    {
        int lheight = height(T->left);
        int rheight = height(T->right);
        if (lheight > rheight)
            return(lheight+1);
        else return(rheight+1);
    }
}

//printing Tree
void printTree(Heap * T)
{
    int h = height(T);
    int i;
    for (i=1; i<=h; i++)
        printGivenLevel(T, i);
}

//Select Vetrex for heap
void printHeap(Heap ** A){
    for (int i = 0; i < cnt; i++)
    {
        if (A[i] != NULL)
        {
            printf("The repeator is %d : ",A[i]->V);
            printTree(A[i]);
            printf("\n");
        }
    }
}

void printBestPath(int T, int R){
    int repeator = R;
    Vertex * findTemp = head;
    printf("%d <- ",repeator);
    while (1)
    {
        if (findTemp->vertex == repeator)
        {
            // printf("Came here .\n");
            if (findTemp->cameFrom == -1)
            {
                printf("Best Path for Traversal\n");
                return;
            }
            printf("%d <- ",findTemp->cameFrom);
            repeator = findTemp->cameFrom;
            findTemp = head;
            continue;
        }
        findTemp = findTemp->next;
    }
}

//Rearrange min after deletion
void rearrangeMIN(Heap * A, int n){
    if (A->right != NULL && A->left != NULL )
    {
        if (A->left->Weight - A->right->Weight > 0)
        {
            if (A->Weight <  A->left->Weight && A->left->visited < n)
            {
                swap(A,A->left);
                rearrangeMIN(A->left,n);
            }
        }
        else
        {
            if (A->Weight < A->right->Weight && A->right->visited < n)
            {
                swap(A,A->right);
                rearrangeMIN(A->right,n);
            }
        }
    }
    else if (A->left != NULL && A->right == NULL && A->left->visited < n){
        if (A->Weight < A->left->Weight)
        {
            swap(A,A->left);
        }
    }
}

//Rearrange max after deletion for next vertex
void rearrangeMAX(Heap * A, int n){
    if (A->right != NULL && A->left != NULL )
    {
        if (A->left->Weight - A->right->Weight < 0)
        {
            if (A->Weight >  A->left->Weight && A->left->visited < n)
            {
                swap(A,A->left);
                rearrangeMAX(A->left,n);
            }
        }
        else
        {
            if (A->Weight > A->right->Weight && A->right->visited < n)
            {
                swap(A,A->right);
                rearrangeMAX(A->right,n);
            }
        }
    }
    else if (A->left != NULL && A->right == NULL && A->left->visited < n){
        if (A->Weight > A->left->Weight)
        {
            swap(A,A->left);
        }
    }
}

Heap * top;
Heap * deletion(Heap * A, int del){
    int k = count(A->left,del);
    int s = count(A->right,del);
    if (k == 0 && s == 0 && A == top)
    {
        A->visited += 1;
        return A;
    }
    else if (k == 0 && s == 0 && A != top)
    {
        swap(A,top);
        A->visited += 1;
        if (del == 1)
            rearrangeMIN(top,del);
        else if (del == 2)
            rearrangeMAX(top,del);
        return top;
    }
    else if (k == s){
        deletion(A->right,del);
    }
    else if (k > s && FullTree(s) == 1){
        deletion(A->left,del);
    }
    else if (k > s && FullTree(s) == 0){
        if (s == 0){
            deletion(A->left,del);
        }
        else{
            deletion(A->right,del);
        }
    }
}

//Update the node of the heap by compairing it's weight with source nodes weight + travel weigth
Vertex * checkUpdate(int vertexS, int vertexD, int weight){
    Vertex * fixedS = head;
    Vertex * V1 = NULL;
    Vertex * V2 = NULL;
    while (fixedS != NULL)
    {
       if (fixedS->vertex == vertexS && vertexS == vertexD)
       {

       }
       else if (fixedS->vertex == vertexS)
       {
           V1 = fixedS;
           if (V2 != NULL)
                break;
       }
       else if (fixedS->vertex == vertexD)
       {
           V2 = fixedS;
           if (V1 != NULL)
                break;
       }
       fixedS = fixedS->next;
    }
    if (V2->fixedWeight > (V1->fixedWeight + weight) && V2->visitStatus != 'F')
    {
        V2->cameFrom = V1->vertex;
        V2->fixedWeight = V1->fixedWeight + weight;
        V2->visitStatus = 'P';
    }
    return V2;
}

//has deletion(heap) && check update;
Heap * updateNext(Heap ** H, int vertex){
    Heap * next ;
    Vertex * select;
    for (int i = 0; i < cnt; i++)
    {
        if (H[i]->V == vertex)
        {
            next = H[i];
        }
    }
    while (next->visited != 1)
    {
        checkUpdate(next->V,next->D,next->Weight);
        top = next;
        next = deletion(next,1);
    }
    // printHeap(H);
    while (1)
    {
        select = head;
        while (select != NULL)
        {

            // printf("%d 1 Successful %d %c .\n",select->vertex, next->V,select->visitStatus);
            if (select->vertex == next->D && select->visitStatus != 'F')
            {
                // printf("\n%d is gone next.\n",select->vertex);
                select->visitStatus = 'F';
                return updateNext(H, select->vertex);
            }
            select = select->next;
        }
        top = next;
        next = deletion(next,2);
        if (next->visited == 2)
            return NULL;
        else{
            // printf("\n%d is gone next1.\n",next->D);
            continue;
        }
    }

}

int main()
{
    //At present Vertex and Edges
    int v[5] = {0,1,2,3,4};
    int edges[13][3] = {{0,1,13}, {0,3,11}, {4,2,15}, {4,3,14}, {0,2,12}, {1,3,11}, {2,4,12}, {2,0,13}, {2,3,14}, {1,3,12}, {3,4,15}, {3,1,12}};
    int vertexCount = 0, edgeCount = 0,new,source,destination,weight,n,transmitor,receiver;
    char resp;

    //Insertion of the Vetex that are default
    for (int i = 0; i < 5; i++)
    {
        createVertex(v[i]);
    }
    printVertex();

    //Deletion of vertex
    while (1)
    {
        printf("\nDo company has removed any Regenerative Repeater(Y : Yes, N : No) : ");
        scanf(" %c",&resp);
        if (resp == 'Y' || resp == 'y')
        {
            printf("Enter the Regenerative Repeater number you want to remove : ");
            scanf("%d",&n);
            deleteVertex(n);
        }
        else if (resp == 'N' || resp == 'n'){
            printf("Moving forward !\n");
            break;
        }
        else{
            printf("Considered as No !\n");
            break;
        }
    }
    printVertex();

    //Insertion in new Vertex
    while (1)
    {
        printf("\nDo company has installed any new Regenerative Repeater(Y : Yes, N : No) : ");
        scanf(" %c",&resp);
        if (resp == 'Y' || resp == 'y')
        {
            printf("Enter the Regenerative Repeater number : ");
            scanf("%d",&new);
            if (checkValidity(new,head->vertex) != 1 ){
                createVertex(new);  //Insertion of new vertex
                enterEdge(new);     //Creation of the new edges for that vertex
            }
            else
            {
                printf(" *** Dont Enter the node that is already installed ! ***\n");
            }

        }
        else if (resp == 'N' || resp == 'n'){
            printf("Moving forward !\n");
            break;
        }
        else{
            printf("Considered as No !\n");
            break;
        }
    }
    printVertex();

    //Insertion of previous edges
    for (int i = 0; i < 13; i++)
    {
        n = checkValidity(edges[i][0],edges[i][1]);  //check that te vertex are present
        if (n == 1){
            addEdge(edges[i][0],edges[i][1],edges[i][2]);
        }
    }
    printEdge();

    //Deletion of the edge
    while (1)
    {
        printf("\nDo company has removed link a link in any two Repeater(Y : Yes, N : No) : ");
        scanf(" %c",&resp);
        if (resp == 'Y' || resp == 'y')
        {
            printf("Enter the link : \n");
            printf("Source : ");
            scanf("%d",&source);
            printf("Destination : ");
            scanf("%d",&destination);
            deleteEdge(source,destination);
        }
        else if (resp == 'N' || resp == 'n'){
            printf("Moving forward !\n");
            break;
        }
        else{
            printf("Considered as No !\n");
            break;
        }
    }

    //The new edge creation and the updation of the old edge weights
    while (1)
    {
        printf("\nDo company has new link or has updated a link in any two Repeater(Y : Yes, N : No) : ");
        scanf(" %c",&resp);
        if (resp == 'Y' || resp == 'y')
        {
            printf("Enter the link : \n");
            printf("Source : ");
            scanf("%d",&source);
            printf("Destination : ");
            scanf("%d",&destination);
            printf("Weight : ");
            scanf("%d",&weight);
            n = checkValidity(source,destination);  //check that te vertex are present
            if (n == 1)
                addEdge(source,destination,weight);
            else if (n == 2)
                printf("The Source repeator is not still installed for Linking.\n");
            else if (n == 3)
                printf("The Destination repeator is not still installed for Linking.\n");
            else if (n == 4)
                printf("The Destination and Source repeator is not still installed for Linking.\n");
        }
        else if (resp == 'N' || resp == 'n'){
            printf("Moving forward !\n");
            break;
        }
        else{
            printf("Considered as No !\n");
            break;
        }
    }

    printEdge();
    printf("\nThe Final Results are : \n");
    printf("\n\n");
    printVertex();
    printf("\n\n");
    printEdge();

    printf("The number of repeators is %d.\n",cnt);
    printf("\n\n");

    Heap * Vheap[cnt];
    for (int i = 0; i < cnt; i++)
    {
        Vheap[i] = NULL;
    }

    //Insertion of the Vertex and Egde in the heap.
    Edge * temp = start;
    while (temp != NULL)
    {
        addToHeap(Vheap,temp->source,temp->destination,temp->weight);
        temp = temp->next;
    }
    printHeap(Vheap);
    printf("\n\n");
    //Select the Transmittor and Receiver.
    while (1)
    {
        printf("Enter the Transmittor : ");
        scanf("%d",&transmitor);
        printf("Enter the Receiver : ");
        scanf("%d",&receiver);
        if (checkValidity(transmitor,receiver) == 1)
            break;
        else if (checkValidity(transmitor,receiver) == 2)
            printf(" *** Enter the valid Receiver. *** \n");
        else if (checkValidity(transmitor,receiver) == 3)
            printf(" *** Enter the valid Transmittor. *** \n");
        else
            printf(" *** Enter the valid Transmittor and Receiver. *** \n");
    }

    printf("\n\n");
    Vertex * fixTemp = head;
    while (fixTemp != NULL)
    {
        printf("%d %f %c \n",fixTemp->vertex,fixTemp->fixedWeight,fixTemp->visitStatus);
        fixTemp = fixTemp->next;
    }

    // checkUpdate(vertex, weight)
    fixTemp = head;
    while (fixTemp != NULL)
    {
        if (fixTemp->vertex == transmitor)
        {
            fixTemp->cameFrom = -1;
            fixTemp->fixedWeight = 0;
            fixTemp->visitStatus = 'F';
        }
        fixTemp = fixTemp->next;
    }
    updateNext(Vheap,transmitor);

    printf("\n\n");
    //Final Result
    fixTemp = head;
    while (fixTemp != NULL)
    {
        printf("%d %f %c %d\n",fixTemp->vertex,fixTemp->fixedWeight,fixTemp->visitStatus,fixTemp->cameFrom);
        fixTemp = fixTemp->next;
    }

    //Best Path from transmitor to receiver
    printf("\n\n");
    printBestPath(transmitor, receiver);
    printf("\n\n");

}
