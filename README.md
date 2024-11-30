# 2.7.10.cublas\<t\>trsm()

## 接口
```
cublasStatus_t cublasStrsm(cublasHandle_t handle,
                           cublasSideMode_t side, cublasFillMode_t uplo,
                           cublasOperation_t trans, cublasDiagType_t diag,
                           int m, int n,
                           const float           *alpha,
                           const float           *A, int lda,
                           float           *B, int ldb)
cublasStatus_t cublasDtrsm(cublasHandle_t handle,
                           cublasSideMode_t side, cublasFillMode_t uplo,
                           cublasOperation_t trans, cublasDiagType_t diag,
                           int m, int n,
                           const double          *alpha,
                           const double          *A, int lda,
                           double          *B, int ldb)
cublasStatus_t cublasCtrsm(cublasHandle_t handle,
                           cublasSideMode_t side, cublasFillMode_t uplo,
                           cublasOperation_t trans, cublasDiagType_t diag,
                           int m, int n,
                           const cuComplex       *alpha,
                           const cuComplex       *A, int lda,
                           cuComplex       *B, int ldb)
cublasStatus_t cublasZtrsm(cublasHandle_t handle,
                           cublasSideMode_t side, cublasFillMode_t uplo,
                           cublasOperation_t trans, cublasDiagType_t diag,
                           int m, int n,
                           const cuDoubleComplex *alpha,
                           const cuDoubleComplex *A, int lda,
                           cuDoubleComplex *B, int ldb)
```

## 数据类型
* S: `float`单精度实数
* D: `double`双精度实数
* C: `cuComplex`单精度复数，输入为单精度复数，输出为单精度复数。
* Z: `cuDoubleComplex`双精度复数，输入为双精度复数，输出为双精度复数。

## 参数说明
| 参数   | 地址类型      | 输入/输出 | 说明                                                                                                                         |
|--------|---------------|-----------|----------------------------------------------------------------------------------------------------------------------------|
| handle |               | 输入      | cuBLAS库上下文句柄。                                                                                                        |
| side   |               | 输入      | indicates if matrix A is on the left or right of X.                                                                        |
| uplo   |               | 输入      | 表明矩阵 A 是存储其下部还是上部，另一部分不被引用，并从存储的元素中推断出来。                                                                                   |
| trans  |               | 输入      | 操作 op(A)，该操作是非转置或（共轭）转置。                                                                                                   |
| diag   |               | 输入      | 指示矩阵 A 的主对角线上的元素是否为单位元素，并且不应被访问。                                                                                           |
| m      |               | 输入      | number of rows of matrix B, with matrix A sized accordingly.                                                               |
| n      |               | 输入      | number of columns of matrix B, with matrix A is sized accordingly.                                                         |
| alpha  | host 或 device| 输入      | <type> scalar used for multiplication, if alpha==0 then A is not referenced and B does not have to be a valid  输入 .        |
| A      | device        | 输入      | <type> array of dimension lda x m with lda>=max(1,m) if side == CUBLAS_SIDE_LEFT and lda x n with lda>=max(1,n) otherwise. |
| lda    |               | 输入      | 用于存储矩阵 A 的二维数组的主导维度。                                                                                                       |
| B      | device        | 输入/输出 | <type> array. It has dimensions ldb x n with ldb>=max(1,m).                                                                |
| ldb    |               | 输入      | 用于存储矩阵 B 的二维数组的主导维度。                                                                                                       |

## 功能说明
这个函数用于解决具有多个right-hand-sides的三角线性系统

$$
\begin{cases}
op(A)X = \alpha B & \text{ if side == {CUBLAS_SIDE_LEFT}} \\
Xop(A) = \alpha B & \text{ if side == {CUBLAS_SIDE_RIGHT}} \\
\end{cases}
$$

其中，$\alpha$和$\beta$是标量，其中A是以下三角或上三角模式存储的三角矩阵（可以包含或不包含主对角线），X和B是维度为m x n的矩阵

对于矩阵A：

$$
\text{op}(A) = \begin{cases}
A & \text{ if transa == {CUBLAS_OP_N}} \\
A^{T} & \text{ if transa == {CUBLAS_OP_T}} \\
A^{H} & \text{ if transa == {CUBLAS_OP_C}} \\
\end{cases}
$$

这个函数在计算完后将解X覆盖在右端项B上。该函数中未包含检测奇异性或接近奇异性的测试。
