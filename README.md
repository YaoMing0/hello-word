# hello-word
#include <stdio.h>
#include <stdlib.h>

typedef struct bgEdge
{
    int v, w;
}Edge;

typedef struct bgStack
{
    Edge* data;
    int top;
}BGStack;
typedef BGStack* Stack;

int matrix[8][8] = {0};

int edges[20][2] = 
{
    {0, 2}, {2, 0},
    {0, 5}, {5, 0},
    {0, 7}, {7, 0},
    {2, 6}, {6, 2},
    {5, 3}, {3, 5},
    {5, 4}, {4, 5},
    {7, 1}, {1, 7},
    {7, 4}, {4, 7},
    {6, 4}, {4, 6},
    {3, 4}, {4, 3},
};

int pre[8] = {-1, -1, -1, -1, -1, -1, -1, -1};
int st[8] = {-1, -1, -1, -1, -1, -1, -1, -1};
int cnt = 0;

Stack stack;

void initStack(int length);

void pushStack(Edge e);

Edge popStack(void);

int isStackEmpty(void);

int isStackFull(void);

void dfs(Edge e);

int main(void)
{
    Edge t;
    int i = 0;

    initStack(100);

    for (i = 0; i < 20; i++)
    {
        matrix[edges[i][0]][edges[i][1]] = 1;
    }

    t.v = 0;
    t.w = 0;

    dfs(t);

    return 0;
}

void dfs(Edge e)
{ 
    Edge t;
    int v;

    pushStack(e);

    while (!isStackEmpty())
    {
        if (pre[(e = popStack()).w] == -1)
        {
            pre[e.w] = cnt++; 
            st[e.w] = e.v;

            for (v = 0; v < 8; v++)
            {
                if (matrix[e.w][v] == 1)
                {
                    if (pre[v] == -1)
                    {
                        t.v = e.w;
                        t.w = v;
                        pushStack(t);
                    }
                }
            }
        }
    }
}

void initStack(int length)
{ 
    stack = malloc(sizeof(BGStack));
    
    stack->data = malloc(sizeof(Edge) * length); 
    stack->top = 0; 
}

void pushStack(Edge e)
{ 
    stack->data[stack->top++] = e; 
}


Edge popStack(void)
{
    Edge ret;

    ret = stack->data[--stack->top];

    return ret; 
}

int isStackEmpty(void)
{
    return (stack->top == 0); 
}
