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
