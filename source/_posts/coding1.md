---
title: C语言解决负数计算问题
category: cs
tag: 
 - C language
 - coding
id: 3
date: 2023-3-29
---

力扣第772题，递归实现简单的计算器，包括加减乘括号

```
#include <stdio.h>
#include <stdlib.h>
#include <ctype.h>

long long parseFactor(char * s, long long* pos);
long long parseTerm(char * s, long long* pos);
long long parseExpression(char * s, long long* pos);

long long calculate(char * s){
    long long pos = 0;
    return parseExpression(s, &pos);
}
long long parseFactor(char * s, long long * pos) {
    if (s[*pos] == '(') {
        (*pos)++;
        long long val = parseExpression(s, pos);
        (*pos)++;
        return val;
    }
    long long val = 0;
    while (isdigit(s[*pos])) {
        val = val * 10 + (s[*pos] - '0');
        (*pos)++;
    }
    return val;
}

long long parseExpression(char * s, long long * pos) {
    long long lhs = parseTerm(s, pos);
    while (s[*pos] == '+' || s[*pos] == '-') {
        char op = s[*pos];
        (*pos)++;
        long long rhs = parseTerm(s, pos);
        if (op == '+') {
            lhs += rhs;
        } else {
            lhs -= rhs;
        }
    }
    return lhs;
}

long long parseTerm(char * s, long long * pos) {
    long long lhs = parseFactor(s, pos);
    while (s[*pos] == '*') {
        char op = s[*pos];
        (*pos)++;
        long long rhs = parseFactor(s, pos);
        if (op == '*') {
            lhs *= rhs;
        } 
    }
    return lhs;
}


int main() {
    char s[100];
    printf("Please input a expression: ");
    scanf("%s", s);
    printf("%s = %lld\n", s,calculate(s));
    return 0;
}
```