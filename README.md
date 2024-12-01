# 2.7.11.cublas\<t\>trsmBatched()

## 接口
```
cublasStatus_t  cublasStrsmBatched(cublasHandle_t    handle,
                                   cublasSideMode_t  side,
                                   cublasFillMode_t  uplo,
                                   cublasOperation_t trans,
                                   cublasDiagType_t  diag,
                                   int m,
                                   int n,
                                   const float *alpha,
                                   const float *const A[],
                                   int lda,
                                   float *const B[],
                                   int ldb,
                                   int batchCount);
cublasStatus_t  cublasDtrsmBatched(cublasHandle_t    handle,
                                   cublasSideMode_t  side,
                                   cublasFillMode_t  uplo,
                                   cublasOperation_t trans,
                                   cublasDiagType_t  diag,
                                   int m,
                                   int n,
                                   const double *alpha,
                                   const double *const A[],
                                   int lda,
                                   double *const B[],
                                   int ldb,
                                   int batchCount);
cublasStatus_t  cublasCtrsmBatched(cublasHandle_t    handle,
                                   cublasSideMode_t  side,
                                   cublasFillMode_t  uplo,
                                   cublasOperation_t trans,
                                   cublasDiagType_t  diag,
                                   int m,
                                   int n,
                                   const cuComplex *alpha,
                                   const cuComplex *const A[],
                                   int lda,
                                   cuComplex *const B[],
                                   int ldb,
                                   int batchCount);
cublasStatus_t  cublasZtrsmBatched(cublasHandle_t    handle,
                                   cublasSideMode_t  side,
                                   cublasFillMode_t  uplo,
                                   cublasOperation_t trans,
                                   cublasDiagType_t  diag,
                                   int m,
                                   int n,
                                   const cuDoubleComplex *alpha,
                                   const cuDoubleComplex *const A[],
                                   int lda,
                                   cuDoubleComplex *const B[],
                                   int ldb,
                                   int batchCount);
```
## 数据类型
* S: `float`单精度实数
* D: `double`双精度实数
* C: `cuComplex`单精度复数，输入为单精度复数，输出为单精度复数。
* Z: `cuDoubleComplex`双精度复数，输入为双精度复数，输出为双精度复数。
## 参数说明
|参数|地址类型|输入/输出|说明|
|-|-|-|-|
|handle|| 输入 |cuBLAS库上下文句柄。|
|side|| 输入 |indicates if matrix A[i] is on the left or right of X[i].|
|uplo|| 输入 |indicates if matrix A[i] lower or upper part is stored, the other part is not referenced and is inferred from the stored elements.|
|trans|| 输入 | 操作 op(A[i])，该操作是非转置或（共轭）转置。|
|diag|| 输入 |indicates if the elements on the main diagonal of matrix A[i] are unity and should not be accessed.|
|m|| 输入 |number of rows of matrix B[i], with matrix A[i] sized accordingly.|
|n|| 输入 |number of columns of matrix B[i], with matrix A[i] is sized accordingly.|
|alpha| host 或 device | 输入 |<type> scalar used for multiplication, if alpha==0 then A[i] is not referenced and B[i] does not have to be a valid  输入 .|
|A|device| 输入 |array of pointers to <type> array, with each array of dim. lda x m with lda>=max(1,m) if side == CUBLAS_SIDE_LEFT and lda x n with lda>=max(1,n) otherwise.|
|lda|| 输入 |leading dimension of two-dimensional array used to store matrix A[i].|
|B|device| 输入/输出 |array of pointers to <type> array, with each array of dim. ldb x n with ldb>=max(1,m). Matrices B[i] should not overlap; otherwise, undefined behavior is expected.|
|ldb|| 输入 |leading dimension of two-dimensional array used to store matrix B[i].|
|batchCount|| 输入 |number of pointers contained in A and B.|
## 功能说明
这个函数解决了一个包含多个右侧项的三角线性系统数组
$\begin{cases}
op(A[i])X[i] = \alpha B[i]  & \text{ if side == {CUBLAS_SIDE_LEFT}} \\
X[i] op(A[i] ) = \alpha B[i]  & \text{ if side == {CUBLAS_SIDE_RIGHT}} \\
\end{cases}$
其中，$\alpha$和$\beta$是标量，其中A[i]是以 lower 或 upper 模式存储的三角矩阵（可以包含或不包含主对角线），X[i]和B[i]是维度为m x n的矩阵
对于矩阵A：

$\text{ op}(A) = \begin{cases}
A \& \text{ if transa == {CUBLAS_OP_N}} \\
A^{T} \& \text{ if transa == {CUBLAS_OP_T}} \\
A^{H} \& \text{ if transa == {CUBLAS_OP_C}} \\
\end{cases}$

这个函数在计算完后将解X[I]覆盖在右端项B[I]上
该函数中未包含检测奇异性或接近奇异性的测试
