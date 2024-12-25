#include <stdio.h>
#include <stdlib.h>
void multiply(int n, int A[][n], int B[][n], int C[][n]);
void strassen(int n, int A[][n / 2], int B[][n / 2], int C[][n / 2]);
void split(int n, int M[][n], int C[][n / 2], int iB, int jB);
void join(int n, int C[][n], int M[][n / 2], int iB, int jB);
void add(int n, int A[][n], int B[][n], int C[][n]);
void subtract(int n, int A[][n], int B[][n], int C[][n]);
void printMatrix(int n, int M[][n]);
int main() {
    int n;
    printf("Enter the dimension of the matrices (n x n): ");
    scanf("%d", &n);
    if (n <= 2)  {
        printf("Performing matrix multiplication.\n");
        int A[n][n], B[n][n], C[n][n];
        printf("Enter matrix A:\n");
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                scanf("%d", &A[i][j]);
        printf("Enter matrix B:\n");
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                scanf("%d", &B[i][j]);        
        printf("\nMatrix A:\n");
        printMatrix(n, A);
        printf("Matrix B:\n");
        printMatrix(n, B);
        multiply(n, A, B, C);
        printf("Resultant matrix:\n");
        //printMatrix(n, C);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++)  {
                printf("%d\t", C[i][j]);
            }
            printf("\n");
        }
    } 
    else  {
        printf("Performing matrix multiplication using Strassen's algorithm.\n");
        // Implement Strassen's algorithm for 4x4 matrices
        int A[n][n], B[n][n], C[n][n];
        printf("Enter matrix A:\n");
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                scanf("%d", &A[i][j]);
        printf("Enter matrix B:\n");
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                scanf("%d", &B[i][j]);
        printf("\nMatrix A:\n");
        printMatrix(n, A);
        printf("Matrix B:\n");
        printMatrix(n, B);
        printf("\n\t\t\t\tSteps to perform Strassen's matrix mltiplication\n");
        strassen(n, A, B, C);
        printf("Resultant matrix:\n");
        //printMatrix(4, C);
        for (int i = 0; i < n; i++)  {
            for (int j = 0; j < n; j++)   {
                printf("%d\t", C[i][j]);
            }
            printf("\n");
        }
    }
    return 0;
}
void multiply(int n, int A[][n], int B[][n], int C[][n]) {
    for (int i = 0; i < n; i++)  {
        for (int j = 0; j < n; j++)   {
            C[i][j] = 0;
            for (int k = 0; k < n; k++)  {
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }
}
void add(int n, int A[][n], int B[][n], int C[][n]) {
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            C[i][j] = A[i][j] + B[i][j];
}
void subtract(int n, int A[][n], int B[][n], int C[][n]) {
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            C[i][j] = A[i][j] - B[i][j];
}
void split(int n, int M[][n], int C[][n / 2], int iB, int jB) {
    for (int i1 = 0, i2 = iB; i1 < n / 2; i1++, i2++)
        for (int j1 = 0, j2 = jB; j1 < n / 2; j1++, j2++)
            C[i1][j1] = M[i2][j2];
}
void join(int n, int C[][n], int M[][n / 2], int iB, int jB) {
    for (int i1 = 0, i2 = iB; i1 < n / 2; i1++, i2++)
        for (int j1 = 0, j2 = jB; j1 < n / 2; j1++, j2++)
            C[i2][j2] = M[i1][j1];
}
void printMatrix(int n, int M[][n]) {
    for (int i = 0; i < n; i++)   {
        for (int j = 0; j < n; j++)   {
            printf("%d\t", M[i][j]);
        }
        printf("\n");
    }
    printf("\n");
}
void strassen(int n, int A[][n / 2], int B[][n / 2], int C[][n /2]) {
    if (n == 2)   {
        multiply(n, A, B, C);
    } 
    else   {
        int n2 = n / 2;
        int A11[n2][n2], A12[n2][n2], A21[n2][n2], A22[n2][n2];
        int B11[n2][n2], B12[n2][n2], B21[n2][n2], B22[n2][n2];
        int C11[n2][n2], C12[n2][n2], C21[n2][n2], C22[n2][n2];
        split(n, A, A11, 0, 0);
        split(n, A, A12, 0, n2);
        split(n, A, A21, n2, 0);
        split(n, A, A22, n2, n2);
        split(n, B, B11, 0, 0);
        split(n, B, B12, 0, n2);
        split(n, B, B21, n2, 0);
        split(n, B, B22, n2, n2);
        printf("\nDividing first matrix into submatrices:\n");
        printf("Matrix A11:\n");
        printMatrix(n / 2, A11);
        printf("Matrix A12:\n");
        printMatrix(n / 2, A12);
        printf("Matrix A21:\n");
        printMatrix(n / 2, A21);
        printf("Matrix A22:\n");
        printMatrix(n / 2, A22);
        printf("\nDividing second matrix into submatrices:\n");
        printf("Matrix B11:\n");
        printMatrix(n / 2, B11);
        printf("Matrix B12:\n");
        printMatrix(n / 2, B12);
        printf("Matrix B21:\n");
        printMatrix(n / 2, B21);
        printf("Matrix B22:\n");
        printMatrix(n / 2, B22);
        int P[n2][n2], Q[n2][n2], R[n2][n2], S[n2][n2];
        int T[n2][n2], U[n2][n2], V[n2][n2];
        int temp1[n2][n2], temp2[n2][n2];
        add(n2, A11, A22, temp1);
        add(n2, B11, B22, temp2);
        strassen(n2, temp1, temp2, P);
        printf("\n\nCalculating the values of P, Q, R, S, T, U & V:\n");
        printf("P = (A11 + A22)*(B11 + B22)\n");
        printf("Matrix P:\n");
        printMatrix(n / 2, P);
        add(n2, A21, A22, temp1);
        strassen(n2, temp1, B11, Q);
        printf("Q = (A21 + A22)*B11\n");
        printf("Matrix Q:\n");
        printMatrix(n / 2, Q);
        subtract(n2, B12, B22, temp1);
        strassen(n2, A11, temp1, R);
        printf("R = A11*(B12 - B22)\n");
        printf("Matrix R:\n");
        printMatrix(n / 2, R);
        subtract(n2, B21, B11, temp1);
        strassen(n2, A22, temp1, S);
        printf("S = A22*(B21 - B11)\n");
        printf("Matrix S:\n");
        printMatrix(n / 2, S);
        add(n2, A11, A12, temp1);
        strassen(n2, temp1, B22, T);
        printf("T = (A11 + A12)*B22\n");
        printf("Matrix T:\n");
        printMatrix(n / 2, T);
        subtract(n2, A21, A11, temp1);
        add(n2, B11, B12, temp2);
        strassen(n2, temp1, temp2, U);
        printf("U = (A21 - A11)*(B11 + B12)\n");
        printf("Matrix U:\n");
        printMatrix(n / 2, U);
        subtract(n2, A12, A22, temp1);
        add(n2, B21, B22, temp2);
        strassen(n2, temp1, temp2, V);
        printf("V = (A12 - A22)*(B21 + B22)\n");
        printf("Matrix V:\n");
        printMatrix(n / 2, V);
        add(n2, P, S, temp1);
        subtract(n2, temp1, T, temp2);
        add(n2, temp2, V, C11);
        printf("\n\nElements of resultant matrix are:\n");
        printf("C11 = P + S - T + V\n");
        printf("Matrix C11:\n");
        printMatrix(n / 2, C11);
        add(n2, R, T, C12);
        printf("C12 = R + T\n");
        printf("Matrix C12:\n");
        printMatrix(n / 2, C12);
        add(n2, Q, S, C21);
        printf("C21 = Q + S\n");
        printf("Matrix C21:\n");
        printMatrix(n / 2, C21);
        add(n2, P, R, temp1);
        subtract(n2, temp1, Q, temp2);
        add(n2, temp2, U, C22);
        printf("C22 = P + R - Q + U\n");
        printf("Matrix C22:\n");
        printMatrix(n / 2, C22);
        join(n, C, C11, 0, 0);
        join(n, C, C12, 0, n2);
        join(n, C, C21, n2, 0);
        join(n, C, C22, n2, n2);
    }
}
