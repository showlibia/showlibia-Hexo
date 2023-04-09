---
title: C语言：栈 后缀表达式计算负数
category: cs
tag: 
 - datastructure
id: 4
date: 2023-3-31
---

在学习栈之后，将中缀表达式转换为后缀表达式时，书本上的代码无法解决负数带来的问题，为此，进行查阅资料后对原代码进行了修改，使其能计算负数。


这篇笔记中的代码使用了C语言。
对于负数有以下两种方法：
- 在负号前加上0，把原负数-n转化为0-n
- 用某个特殊符号对负数进行标记

这段代码使用的是第一种方法
```

#define MAX_SIZE 100 

typedef struct {
    int top;
    char data[MAX_SIZE];
} Stack; 

typedef struct{
    int top;
    long long data[MAX_SIZE];
} Stack1; 

void initStack(Stack *&s) {
    s = (Stack *)malloc(sizeof(Stack));
    s->top = -1;
}

void initStack1(Stack1 *&s) {
    s = (Stack1 *)malloc(sizeof(Stack1));
    s->top = -1;
}

void DestroyStack(Stack *&s) {
    free(s);
}

void DestroyStack1(Stack1 *&s) {
    free(s);
}

bool Push(Stack *&s, char e) {
    if (s->top == MAX_SIZE - 1) {
        return false;
    }
    s->data[++s->top] = e;
    return true;
}

bool Push1(Stack1 *&s, long long e) {
    if (s->top == MAX_SIZE - 1) {
        return false;
    }
    s->data[++s->top] = e;
    return true;
}

bool Pop(Stack *&s, char &e) {
    if (s->top == -1) {
        return false;
    }
    e = s->data[s->top--];
    return true;
}

bool Pop1(Stack1 *&s, long long &e) {
    if (s->top == -1) {
        return false;
    }
    e = s->data[s->top--];
    return true;
}

bool StackEmpty(Stack *s) {
        return s->top == -1;
}
bool StackEmpty1(Stack1 *s) {
        return s->top == -1;
}

bool GetTop(Stack *s, char &e) {
    if (s->top == -1) {
        return false;
    }
    e = s->data[s->top];
    return true;
}

bool GetTop1(Stack1 *s, long long &e) {
    if (s->top == -1) {
        return false;
    }
    e = s->data[s->top];
    return true;
}

void trans(char*exp,char*postexp)
{
    char e;
    Stack *Optr;
    initStack(Optr);
    long long i=0;
    while (*exp != '\0')
    {
        switch(*exp)
        {
            case '(':Push(Optr,*exp);exp++;break;
            case ')':Pop(Optr,e);
                    while(e!='(')
                    {
                        postexp[i++]=e;
                        Pop(Optr,e);
                    }
                    exp++;break;
            case '+':
            case '-':
                while (!StackEmpty(Optr))
                {
                    GetTop(Optr,e);
                    if(e!= '(')
                    {
                        postexp[i++]=e;
                        Pop(Optr,e);
                    }
                    else
                    {
                        break;
                    }
                }
                GetTop(Optr,e);
                if (*exp == '-'&& (StackEmpty(Optr)|| e == '('))
                {
                    postexp[i++] = '0';
                    postexp[i++] = '#';
                }
                
                Push(Optr,*exp);
                exp++;
                break;
            case '*':
                while (!StackEmpty(Optr))
                {
                    GetTop(Optr,e);
                    if(e=='*')
                    {
                        postexp[i++]=e;
                        Pop(Optr,e);
                    }
                    else
                    {
                        break;
                    }
                }
                Push(Optr,*exp);
                exp++;
                break;
            default:
                while(isdigit(*exp))
                {
                    postexp[i++]=*exp;
                    exp++;
                }
                postexp[i++]='#';
                break;
        }
    }
    while (!StackEmpty(Optr))
    {
        Pop(Optr,e);
        postexp[i++]=e;
    }
    postexp[i]='\0';
    DestroyStack(Optr);
}

long long compvalue(char * postexp)
{
    long long d,a,b,c,e;
    Stack1 *Opnd;
    initStack1(Opnd);
    while (*postexp != '\0')
    {
        a=0,b=0;
        switch (*postexp)
        {
            case '+':
                Pop1(Opnd,b);
                Pop1(Opnd,a);
                c=a+b;
                Push1(Opnd,c);
                break;
            case '-':
                Pop1(Opnd,b);
                Pop1(Opnd,a);
                c=a-b;
                Push1(Opnd,c);
            
                break;
            case '*':
                Pop1(Opnd,b);
                Pop1(Opnd,a);
                c=a*b;
                Push1(Opnd,c);
                break;
            default:
                d=0;
                while(isdigit(*postexp))
                {
                    d=d*10+*postexp-'0';
                    postexp++;
                }

                Push1(Opnd,d);
                break;
        }
        postexp++;
    }
    GetTop1(Opnd,e);
    DestroyStack1(Opnd);
    return e;
}
```
