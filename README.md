hello
=====
this Gorang Maniar
Student
IIIT-Hyderabad

prob1: 
There is a man who wants to climb a rock from a starting point to the destination point. Given a map of the rock mountain which N = height, M = width. In the map, character '-' is the possible foot place spot (where can climb).
He can freely move up/down at vertical spots which '-' exists sequentially. It's impossible to move horizontally in case there is more than one space between '-' in the same height level.
Depending on how high/low he moves towards the upper or lower direction at one time, the level of difficulty of rock climbing gets determined.
The maximum height of moving from the starting point to destination point is the level of difficulty of rock climbing .
The total distance of movement is not important. There are more than one path from the starting point to destination point. => Output: The minimum level of difficulty of all rock climbing paths level.
Hint: Start with difficulty level 0 and then keep increasing it one by one.
int matrix[MAX_SIZE][MAX_SIZE];
int visited_matrix[MAX_SIZE][MAX_SIZE];

int move[4][2] = {
	1, 0,
	-1, 0,
	0, -1,
	0, 1
};

int check_for_level(int dest_x, int dest_y);
int check_for_last_row(int dest_x, int dest_y, int level);

int row = 0;
int col = 0;

int prob(int x)
{
	if (rand() % 100 < x)
		return 1;
	else
		return 0;
}

void init_visited_matrix()
{
	int i, j;
	for (i = 0; i < MAX_SIZE; i++)
	for (j = 0; j < MAX_SIZE; j++)
		visited_matrix[i][j] = 0;
}

void print_matrix(int m, int n)
{
	int i, j;
	for (i = 0; i < m; i++)
	{
		for (j = 0; j < n; j++)
			printf("%d ", matrix[i][j]);
		printf("\n");
	}
}

int main()
{
	srand(10);
	int i, j, m, n, p, t, ans, loc_x, loc_y;
	int tc = 50;
	start = clock();
	for (t = 0; t < tc; t++)
	{
		m = 20 + rand() % 30;
		n = 20 + rand() % 30;

		//For safety, fill the whole matrix with -1 to start with
		for (i = 0; i < m; i++)
		for (j = 0; j < n; j++)
			matrix[i][j] = -1;


		//Fill the left corner with 2
		matrix[m - 1][0] = 2;

		//Fill the last line with all 1
		for (i = 1; i < n; i++)
			matrix[m - 1][i] = 1;

		//Pick a random location and fill it with 3
		loc_x = rand() % m;
		loc_y = rand() % n;
		matrix[loc_x][loc_y] = 3;

		//pick a random probability between 40 to 60 to fill with 1
		p = 0 + rand() % 20;

		//fill the rest of the matrix with 1s and 0s with probability p

		for (i = 0; i < m; i++)
		for (j = 0; j < n; j++)
		{
			if (-1 == matrix[i][j])
			{
				if (prob(p))
					matrix[i][j] = 1;
				else
					matrix[i][j] = 0;
			}
		}
		row = m;
		col = n;
		//print_matrix(m,n);
		
		ans = check_for_level(loc_x, loc_y);
		
		//printf("Row %d Col %d Loc_x %d Loc_y %d\n", m, n, loc_x, loc_y);
		printf("Case#%d: %d\n", t, ans);

	}
	end = clock();
	cpu_time_used = ((double)(end - start));
	printf("%lf\n", cpu_time_used);
	return 0;
}

int check_for_level(int dest_x, int dest_y)
{
	int level = 1;
	while (1)
	{
		init_visited_matrix();
		visited_matrix[dest_x][dest_y] = 1;
		if (1 == check_for_last_row(dest_x, dest_y, level))
		{
			return level;
		}
		level++;
	}
}

int check_for_last_row(int dest_x, int dest_y, int level)
{
	if (dest_x == row-1)
		return 1;

	int k, temp_x, temp_y;
	for (k = 0; k < (2 + 2 * level); k++)
	{
		if (k < 4)
		{
			temp_y = dest_y + move[k][1];
			temp_x = dest_x + move[k][0];
		}
		else
		{
			temp_y = dest_y;
			temp_x = dest_x + (k/2)*move[k%2][0];
		}
		if (temp_x >= 0 && temp_y >= 0 && temp_x < row && temp_y < col)
		{
			if (matrix[temp_x][temp_y] == 1 && visited_matrix[temp_x][temp_y] == 0)
			{
				visited_matrix[temp_x][temp_y] = 1;
				if (1 == check_for_last_row(temp_x, temp_y, level))
					return 1;
			}
		}
	}
	return 0;
}

**************************************************************************************************************************************

prob2:
We have a game where an airplane is placed in the middle column of the bottom row. The airplane can move right or left by one step and in every step the row moves down. When the airplane meets ‘1’ (coin) the number of points increase by 1 and when the airplane meets ‘2’ (bomb) the number of points decrease by 1. Whenever the airplane meets the bomb with score 0 the airplane dies and game is over. The user has one detonate option throughout the game where he can detonate all the bombs in the next 5 rows.  Find the maximum number of points (coins) that can be collected by the user.  Number of rows 1 <= N <= 12. Return -1 if score < 0  
 
1 2 0 0 1 2 0 0 1 0 0 1 2 0 1 1 0 0 2 1 0 2 1 0 1 0 1 2 2 2 1 0 1 1 0   A   
  
     1 2 0 0 1 2 0 0 1 0 0 1 2 0 1 1 0 0 2 1 0 2 1 0 1 0 1 2 2 2   A   
 
     1 2 0 0 1 0 0 0 1 0 0 1 0 0 1 1 0 0 0 1 0 0 1 0 1 0 1 0 0 0   A   
 
                    1 2 0 0 1 0 0 0 1 0 0 1 0 0 1    A  
 
               1 2 0 0 1 0 0 0 1 0 0 1 0 0 1 1 0 0 0 1   A   
 
          1 2 0 0 1 0 0 0 1 0 0 1 0 0 1 1 0 0 0 1 0 0 1 0 1  A    
 
                         1 2 0 0 1 0 0 0 1 0     A 
 
                              1 2 0 0 1    A  
 
                                       A 
 
 #include <iostream> using namespace std; int a[13][5], b[5][5]; 
 
void detonate(int row){     for (int i = row; i > row - 5; i--){         for (int j = 0; j < 5; j++){             b[row - i][j] = 0;             if (i >= 0 && a[i][j] == 2)             {                 b[row - i][j] = 2;                 a[i][j] = 0;             }         }     } } void unDetonate(int row){     for (int i = row; i > row - 5; i--){         for (int j = 0; j < 5; j++)         {             if (i >= 0 && b[row - i][j] == 2)             {                 a[i][j] = 2;             }         }     } } 
 
void getMaxCoins(int pos, int coins, int n, int &maxCoins){          if (pos < 0 || pos > 4 || coins < 0) return;          if (a[n - 1][pos] == 2) coins -= 1;     else if (a[n - 1][pos] == 1) coins += 1; 
 
    if (n == 1){         if (coins > maxCoins) maxCoins = coins;         return;     }     else{         // 3 options         // move right         getMaxCoins(pos + 1, coins, n - 1, maxCoins);         // move left         getMaxCoins(pos - 1, coins, n - 1, maxCoins);         // not move          getMaxCoins(pos, coins, n - 1, maxCoins);     } } 
 
int main(){     int t, n, i, j, k, coins, maxCoins;     cin >> t;     for (i = 0; i < t; i++){         cin >> n;         maxCoins = -1;         for (j = 0; j < n; j++){             for (k = 0; k < 5; k++){                 cin >> a[j][k];             }         }         for (j = 0; j < 5; j++) a[n][j] = 0;         a[n][2] = 3;         for (j = n - 1; j > 0 ; j--)         {             coins = -1;             //detonate             detonate(j);             getMaxCoins(2, 0, n, coins);             if (coins > maxCoins) maxCoins = coins;             unDetonate(j);             // undetonate 
 
        }         cout << ((maxCoins < 0) ? -1 : maxCoins) << endl;     } } 
        
        
   ********************************************************************************************************************     
        
        prob3:
        
        Four 5G base station towers needs to be installed in a  Landscape which is divided as hexagon cells as shown in Fig below, which also contains number of people living in each cell. Need to find four cells  to install the 5G towers which can cover maximum number of people  combining all four cells, with below conditions
Only one tower can be placed in a cell
Each of the four chosen cell should be neighbor to atleast one of the remaining 3 cells. 
All four cells should be connected  (like  one island)

Refer next slide for some valid combinations
Input range:  1 <= N, M <= 15
Sample input Format for Fig in right
3 4
150 450 100 320
120 130 160 110
10 60 200 220

Output
Square of  Maximum number of people covered by 4 towers


#define _CRT_SECURE_NO_WARNINGS

#include <stdio.h>

#define MAX 16

///////////// Method #1

int N, M;
int cells[MAX][MAX];
int visited[MAX][MAX];
int maxcount;

int odir[2][6] = { { 0, 0, -1, 1, 1, 1 },
					{ -1, 1, 0, 1, -1, 0 } };
int edir[2][6] = { { -1, -1, -1, 0, 1, 0 },
					{ -1, 1, 0, 1, 0, -1 } };


int isvalid(int i, int j){
	if (i < 0 || i >= N || j < 0 || j >= M)
		return 0;
	return 1;
}
int findmaxscore(int cX, int cY, int count, int curValue){

	int i, nX, nY;
	if (count == 4){
		if (maxcount < curValue){
			maxcount = curValue;
		}
		return;
	}

	for (i = 0; i < 6; i++){
		if (cY % 2 == 0){
			nX = cX + edir[0][i];
			nY = cY + edir[1][i];
		}
		else{
			nX = cX + odir[0][i];
			nY = cY + odir[1][i];
		}

		if (isvalid(nX, nY) && visited[nX][nY] == 0){
			visited[nX][nY] = 1;
			findmaxscore(cX, cY, count + 1, curValue + cells[nX][nY]);
			findmaxscore(nX, nY, count + 1, curValue + cells[nX][nY]);
			visited[nX][nY] = 0;
		}
	}
}

///////////// Method #2

int Hcells[MAX * 2][MAX];
int visit2[MAX * 2][MAX];
int gmaxcore;

int dir[2][6] = { { -1, 1, 2, 1, -1, -2 },
					{ 1, 1, 0, -1, -1, 0 } };

int issafe(int i, int j){
	if (i < 0 || i >= N * 2 || j < 0 || j >= M)
		return 0;
	return 1;
}
int findmax2(int cX, int cY, int count, int curValue){

	int i, nX, nY;
	if (count == 4){
		if (gmaxcore < curValue){
			gmaxcore = curValue;
		}
		return;
	}

	for (i = 0; i < 6; i++){
		nX = cX + dir[0][i];
		nY = cY + dir[1][i];

		if (issafe(nX, nY) && visit2[nX][nY] == 0){
			visit2[nX][nY] = 1;
			findmax2(cX, cY, count + 1, curValue + Hcells[nX][nY]);
			findmax2(nX, nY, count + 1, curValue + Hcells[nX][nY]);
			visit2[nX][nY] = 0;
		}
	}
}



int main(void)
{
	int T, test_case;

	freopen("input.txt", "r", stdin);

	setbuf(stdout, NULL);
	scanf("%d", &T);

	for (test_case = 0; test_case < T; test_case++)
	{

		/////////////////////////////////////////////////////////////////////////////////////////////
		/*
		Implement your algorithm here.
		The answer to the case will be stored in variable Answer.
		*/
		/////////////////////////////////////////////////////////////////////////////////////////////

		int i, j, k, l;
		scanf("%d %d", &N, &M);

		for (i = 0; i < N; i++){
			for (j = 0; j < M; j++){
				scanf("%d", &cells[i][j]);
			}
		}

		maxcount = 0;
		for (i = 0; i < N; i++){
			for (j = 0; j < M; j++){
				visited[i][j] = 1;
				findmaxscore(i, j, 1, cells[i][j]);
				visited[i][j] = 0;
			}
		}

		//////////// Method 2  ////////////

		int x, y;
		for (i = 0, x = 0; i < N; i++, x += 2){
			for (j = 0; j < M; j++){
				if (j % 2 == 0)
					Hcells[x][j] = cells[i][j];
				else
					Hcells[x + 1][j] = cells[i][j];
			}
		}

		gmaxcore = 0;
		for (i = 0; i < N * 2; i++){
			for (j = 0; j < M; j++){
				if (Hcells[i][j] != 0){
					visit2[i][j] = 1;
					findmax2(i, j, 1, Hcells[i][j]);
					visit2[i][j] = 0;
				}
			}
		}

		// Print the answer to standard output(screen).
		printf("%d\n", maxcount*maxcount);
		printf("%d\n", gmaxcore*gmaxcore);

	}

	return 0;//Your program should return 0 on normal termination.
}
*********************************************************************************************************************************
prob4:
Mr. Choi has to do a marathon of D distance. He can run at 5 different paces, each pace will have its time consumed per km and its energy consumption.
Mr. Choi can only run till he had energy left.
Find the minimum time required for choi to complete marathon if he has H energy.

#define _CRT_SECURE_NO_WARNINGS
#include<iostream>
using namespace std;

int abc[41][601];
int Energy;
int dist;
int gears[5][2];

int min(int x, int g)
{
	int mina = 99999;
	for (int i = 0; i < 5; i++)
	{
		if (g - gears[i][1] > 0)
		if ((abc[x - 1][g - gears[i][1]] + gears[i][0]) < mina)
			mina = abc[x - 1][g - gears[i][1]] + gears[i][0];
	}
	return mina;
}

int main()
{
	//freopen("input1.txt", "r", stdin);
	//freopen("output_our.txt", "w", stdout);
	int tc;
	int a, b;
	int minb = 99999;
	cin >> tc;
	for (int t = 0; t < tc; t++)
	{
		minb = 99999;
		cin >> Energy >> dist;
		for (int i = 0; i < 5; i++)
		{
			cin >> a >> b >> gears[i][1];
			gears[i][0] = a * 60 + b;
		}
		for (int i = 1; i <= Energy; i++)
		for (int j = 0; j <= dist; j++)
		{
			abc[j][i] = 99999;
		}
		for (int i = 1; i <= 5; i++)
		{
			abc[1][gears[i - 1][1]] = gears[i - 1][0];
		}
		for (int i = 2; i <= dist; i++)
		{
			for (int j = 1; j <= Energy; j++)
			{
				abc[i][j] = min(i, j);
			}
		}
		for (int i = 1; i <= Energy; i++)
		{
			if (abc[dist][i] < minb)
				minb = abc[dist][i];
		}
		cout << "#" << t + 1 << " " << minb / 60 << " " << minb % 60 << endl;
	}
	return 0;
}

***********************************************************************************************************************************
prob5: BFS
#include<stdio.h>
#include<stdlib.h>

//  \  "\n"
//   ||
struct adjlistnode{
    int data;
    struct adjlistnode *next;
};
struct adjlist{
    struct adjlistnode *head;
};
struct graph{
    int v;
    struct adjlist *arr;
};
struct qnode{
    int data;
    struct qnode *next;
};
struct queue{
    struct qnode *front;
    struct qnode *rear;
};
struct qnode * newqnode(int data){
    struct qnode *temp=(struct qnode *)malloc(sizeof(struct qnode));
    temp->data=data;
    temp->next=NULL;
    return temp;
}
struct queue *createqueue(){
    struct queue* temp=(struct queue *)malloc(sizeof(struct queue));
    temp->front=NULL;
    temp->rear=NULL;
    return temp;
}
void push_in_queue(struct queue *q,int data){
    struct qnode *temp = newqnode(data);
    if(q->rear==NULL){
        q->front=q->rear=temp;
        return;
    }
    q->rear->next=temp;
    q->rear=temp;
}
struct qnode* pop_from_queue(struct queue *q){
    if(q->front==NULL){
        return NULL;
    }
    struct qnode *temp=q->front;
    q->front=q->front->next;

    if(q->front==NULL){
        q->rear=NULL;
    }
    return temp;
}
int isEmpty(struct queue *q){
    if(q->rear==NULL){
        return 1;
    }
    else{
        return 0;
    }
}
struct adjlistnode* new_adjlistnode(int data){
    struct adjlistnode *temp=(struct adjlistnode*)malloc(sizeof(struct adjlistnode));
    temp->data=data;
    temp->next=NULL;
    return temp;
}
struct graph* creategraph(int v){
    struct graph* temp=(struct graph *)malloc(sizeof(struct graph));
    temp->v=v;
    temp->arr=(struct adjlist *)malloc(v*sizeof(struct adjlist));

    int i;
    for(i=0;i<v;i++){
        temp->arr[i].head=NULL;
    }
    return temp;
}
void addedge(struct graph *g,int src,int dest){
    struct adjlistnode *temp=new_adjlistnode(dest);
    temp->next = g->arr[src].head;
    g->arr[src].head=temp;

    temp=new_adjlistnode(src);
    temp->next=g->arr[dest].head;
    g->arr[dest].head=temp;
}
void BFS(struct graph *g,int s){
    int visited[g->v];
    int i;
    for(i=0;i<g->v;i++){
        visited[i]=0;
    }
    visited[s]=1;

    struct queue *q=createqueue();
    push_in_queue(q,s);

    while(isEmpty(q)==0){
        struct qnode *f= q->front;
        printf("%d\n",f->data);
        pop_from_queue(q);

        struct adjlistnode *t=g->arr[f->data].head;

        while(t!=NULL){
            if(visited[t->data]==0){
                push_in_queue(q,t->data);
                visited[t->data]=1;
            }
            t=t->next;
        }
    }
}
void printGraph(struct graph *g){
    int i;
    for(i=0;i<g->v;i++){
        printf("\n Adjacency list : %d\n head ", i);
        struct adjlistnode *head=g->arr[i].head;
        while(head!=NULL){
            printf("--> %d ",head->data);
            head=head->next;
        }
        printf("\n");
    }
}
int main(){
    int v=5;

    struct graph *g=creategraph(v);
    addedge(g,0,1);
    addedge(g,0,4);
    addedge(g,1,2);
    addedge(g,1,3);
    addedge(g,1,4);
    addedge(g,2,3);
    addedge(g,3,4);

    printGraph(g);

    printf("\n\n");

    BFS(g,2);

}
//   "\n"
**********************************************************************************************************************************

prob6: DFS

#include<stdio.h>
#include<stdlib.h>

//  \  "\n"
//   ||



struct adjlistnode{
    int data;
    struct adjlistnode *next;
};

struct adjlist{
    struct adjlistnode *head;
};

struct graph{
    int v;
    struct adjlist *arr;
};
/*
struct qnode{
    int data;
    struct qnode *next;
};
struct queue{
    struct qnode *front;
    struct qnode *rear;
};
struct qnode * newqnode(int data){
    struct qnode *temp=(struct qnode *)malloc(sizeof(struct qnode));
    temp->data=data;
    temp->next=NULL;
    return temp;
}
struct queue *createqueue(){
    struct queue* temp=(struct queue *)malloc(sizeof(struct queue));
    temp->front=NULL;
    temp->rear=NULL;
    return temp;
}
void push_in_queue(struct queue *q,int data){
    struct qnode *temp = newqnode(data);
    if(q->rear==NULL){
        q->front=q->rear=temp;
        return;
    }
    q->rear->next=temp;
    q->rear=temp;
}
struct qnode* pop_from_queue(struct queue *q){
    if(q->front==NULL){
        return NULL;
    }
    struct qnode *temp=q->front;
    q->front=q->front->next;

    if(q->front==NULL){
        q->rear=NULL;
    }
    return temp;
}
int isEmpty(struct queue *q){
    if(q->rear==NULL){
        return 1;
    }
    else{
        return 0;
    }
}
*/
struct adjlistnode* new_adjlistnode(int data){
    struct adjlistnode *temp=(struct adjlistnode*)malloc(sizeof(struct adjlistnode));
    temp->data=data;
    temp->next=NULL;
    return temp;
}
struct graph* creategraph(int v){
    struct graph* temp=(struct graph *)malloc(sizeof(struct graph));
    temp->v=v;
    temp->arr=(struct adjlist *)malloc(v*sizeof(struct adjlist));

    int i;
    for(i=0;i<v;i++){
        temp->arr[i].head=NULL;
    }
    return temp;
}

void addedge(struct graph *g,int src,int dest){
    struct adjlistnode *temp=new_adjlistnode(dest);
    temp->next = g->arr[src].head;
    g->arr[src].head=temp;

    temp=new_adjlistnode(src);
    temp->next=g->arr[dest].head;
    g->arr[dest].head=temp;
}
/*
void BFS(struct graph *g,int s){
    int visited[g->v];
    int i;
    for(i=0;i<g->v;i++){
        visited[i]=0;
    }
    visited[s]=1;

    struct queue *q=createqueue();
    push_in_queue(q,s);

    while(isEmpty(q)==0){
        struct qnode *f= q->front;
        printf("%d\n",f->data);
        pop_from_queue(q);

        struct adjlistnode *t=g->arr[f->data].head;

        while(t!=NULL){
            if(visited[t->data]==0){
                push_in_queue(q,t->data);
                visited[t->data]=1;
            }
            t=t->next;
        }


    }



}
*/
void dfsutil(struct graph *g,int visited[],int s){
    visited[s]=1;
    printf("%d\n",s);
    struct adjlistnode *t=g->arr[s].head;

    while(t!=NULL){
        if(visited[t->data]==0){
            dfsutil(g,visited,t->data);
        }
        t=t->next;
    }
}

void DFS(struct graph* g,int s){
    int visited[g->v];
    int i;
    for(i=0;i<g->v;i++){
        visited[i]=0;
    }
    dfsutil(g,visited,s);
}

void printGraph(struct graph *g){
    int i;
    for(i=0;i<g->v;i++){
        printf("\n Adjacency list : %d\n head ", i);
        struct adjlistnode *head=g->arr[i].head;
        while(head!=NULL){
            printf("--> %d ",head->data);
            head=head->next;
        }
        printf("\n");

    }

}



int main(){
    int v=5;

    struct graph *g=creategraph(v);
    addedge(g,0,1);
    addedge(g,0,4);
    addedge(g,1,2);
    addedge(g,1,3);
    addedge(g,1,4);
    addedge(g,2,3);
    addedge(g,3,4);

    printGraph(g);

    printf("\n\n");

    DFS(g,2);

}




//   "\n"

*************************************************************************************************************************************
previous question:
#include<bits/stdc++.h>
using namespace std;

int log(long long int n)
{
	int ct=0;
	while(n>1)
	{
		n = n/2;
		ct++;	
	}
	return ct;
}

long long int find(long long int a,long long int b,long long int c,long long int num, long long int mid,long long int min, long long int max)
{
	int lg = log(mid);
	long long int fn =a+(b*lg)+(c*mid*mid);

//	cout<<"mid="<<mid<<" "<<"fn="<<fn<<endl;
	long long int rem = num%mid;
	//temp = temp - (a*mid) - (b*lg*mid);
	//long long int quo = num/mid;
	//cout<<"temp="<<temp<<endl;
	
	if(mid*mid<mid)
	{
		max = mid;
		mid = (min/2)+(max/2);
		find(a,b,c,num,mid,min,max);
	}

	else if(rem!=0 && (num/fn)>mid)
	{
		min = mid;
		//	cout<<"min="<<min<<" "<<"max="<<max<<endl;	
		mid = (min/2)+(max/2);
		//	cout<<"updated_mid="<<mid<<" ";
		find(a,b,c,num,mid,min,max);
	}
	else if(rem!=0 && (num/fn)<mid)
	{
		max = mid;
		mid = (min/2)+(max/2);
		find(a,b,c,num,mid,min,max);
	}
	else if(rem==0 && (num/fn)==mid)
	{
			return mid;
	}
	else
		return -1;



}

int main()
{
	
	for(int i=0;i<10;i++)
	{
		long long int a,b,c,num;
		cin>>a>>b>>c>>num;

		
		long long int min =0, max = INT_MAX;
		max=max-1;
		long long int mid = (min+max)/2,ans=0;
		// cout<<"num="<<num<<endl;
		ans = find(a,b,c,num,mid,min,max);
		if(ans<0)
			cout<<"#"<<i+1<<" "<<ans+1<<endl;
		else
			cout<<"#"<<i+1<<" "<<ans<<endl;
	}
	return 0;
}



***********************************************************************************************************************
fishery problem
#include <iostream>

using namespace std;

int n; // It was less than 60 i think
int gates[3] = {0, 1, 2}; // Stores gates permutation
int fishermen[61]; // Stores if particular position is already taken by a fisherman or not.
int min_ans = 1000000; // Answer
int gate_info[3][2]; // Stores gate wise position and number of fishermen standing in fron of it.

int get_min(int a, int b) {
	return (a < b)?a:b;
}

void min_walk(int index, int walk_till_now, int fishermen[]) {
	if(index >= 3) {
		//cout << walk_till_now << " ";
		min_ans = get_min(min_ans, walk_till_now);
		return;
	}

	int pos = gate_info[gates[index]][0];
	int ppl = gate_info[gates[index]][1];
	//cout << "Position: " << pos << " | Peoples: " << ppl << endl;

	// Fill the first position directly. There after people can go in pair if possible.
	if(fishermen[pos] != 1){
		fishermen[pos] = 1;
		walk_till_now += 1;
		ppl--;
	}

	// j is like radius increasing by one every iteration.
	for(int i = ppl, j = 1; i > 0; j++) {
		// Take care of the case when just one fisherman is left and he can go to either left or right position.
		if(i == 1 && pos - j >= 0 && fishermen[pos - j] != 1 && pos + j < n && fishermen[pos + j] != 1) {
			// Try left position and find its minimum walk.
			int temp[61];
			for(int q = 0; q<61; q++)
				temp[q] = fishermen[q];
			fishermen[pos - j] = 1;
			min_walk(index + 1, walk_till_now + j + 1, fishermen);

			// Backtrack 
			for(int q = 0; q<61; q++)
				fishermen[q] = temp[q];
			
			// Try right position and find its minimum walk.
			fishermen[pos + j] = 1;
			min_walk(index + 1, walk_till_now + j + 1, fishermen);
			i--;
			return;
		}

		// To check if the left position is empty or not. If it is fill it.
		if(pos-j >= 0 && fishermen[pos - j] != 1) {
			fishermen[pos - j] = 1;
			walk_till_now = walk_till_now + j + 1;
			i--;
		}

		// To check if the right position is empty or not. If it is fill it.
		if(pos+j < n && fishermen[pos + j] != 1) {
			fishermen[pos + j] = 1;
			walk_till_now = walk_till_now + j + 1;
			i--;
		}
	}
	// If there was no choice, proceed without any problem.
	min_walk(index + 1, walk_till_now, fishermen);
}

void swap(int i, int j) {
	int temp = gates[i];
	gates[i] = gates[j];
	gates[j] = temp;
}

// Function to find permutation of any m( = 3 here) sized array.
// Otherwise simply give following 6 known possible permutations: {{0, 1, 2}, {0, 2, 1}, {1, 0, 2}, {1, 2, 0}, {2, 1, 0}, {2, 0, 1}} 
void getPerms(int index) {
	if(index >= 3){
		//cout << gates[0] << " " << gates[1] << " " << gates[2] << endl;
		for(int i = 0; i<61; i++)
			fishermen[i] = 0;
		min_walk(0, 0, fishermen);
		return;
	}
	for(int j = index; j < 3; j++) {
		swap(index, j); // Swap i with j
		getPerms(index+1);
		swap(index, j); // Backtrack
	}
}

int main() {
	int testcases;
	cin >> testcases;
	for(int testcase = 0; testcase < testcases; testcase++) {
		cin >> n;		
		for(int i = 0; i < 3; i++) {
			cin >> gate_info[i][0] >> gate_info[i][1];
			gate_info[i][0]--; // Position is 1-indexed, make it 0
		}
		min_ans = 1000000;
		getPerms(0);
		cout << "Min walk: " << min_ans << endl;
	}
}


**********************************************************************************************************************
shooting ballon
int getmaxpoint(int l,int r,int n)
{
  int maxscore=0;
  for(int i=l+1;i<r;i++)
  {
    int current_sum=0;
    current_sum=getmaxpoint(l,i,n)+getmaxpoint(i,r,n);
    if(l==0 && r==n+1)
    current_sum=current_sum+balloon[i];
    else
    current_sum=current_sum+balloon[l]*balloon[r];
    
    if(current_sum>maxscore)
    maxscore=current_sum;
  }
  return maxscore;
}


