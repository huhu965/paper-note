### Eigen::Isometry3d 中关于pretranslate、translate、prerotate、rotate的理解
经过实验测试，这四个都是相当于矩阵的乘法，假设默认矩阵为R，那么带pre的相当于默认矩阵左乘给定的矩阵。如果不带的话，就相当于默认矩阵右乘给定的矩阵。
同时，调用函数的顺序不同，计算出的结果也是不同的。例如：
Eigen::Isometry3d pose;
pose.rotate(R~1~);
pose.translate(t~1~);
假设pose为$\begin{array}{|ll|}
{R}&{t}\\
{0}&{1}\\
\end{array}$,那么上面的代码相当于$\begin{array}{|ll|}
{R}&{t}\\
{0}&{1}\\
\end{array}$ * $\begin{array}{|ll|}
{R_1}&{0}\\
{0}&{1}\\
\end{array}$ * $\begin{array}{|ll|}
{I}&{t_1}\\
{0}&{1}\\
\end{array}$ = $\begin{array}{|ll|}
{R}&{t}\\
{0}&{1}\\
\end{array}$ * $\begin{array}{|ll|}
{R_1}&{R_1*t_1}\\
{0}&{1}\\
\end{array}$  

Eigen::Isometry3d pose;
pose.translate(t~1~);
pose.rotate(R~1~);
相当于$\begin{array}{|ll|}
{R}&{t}\\
{0}&{1}\\
\end{array}$ * $\begin{array}{|ll|}
{I}&{t_1}\\
{0}&{1}\\
\end{array}$ * $\begin{array}{|ll|}
{R_1}&{0}\\
{0}&{1}\\
\end{array}$ = $\begin{array}{|ll|}
{R}&{t}\\
{0}&{1}\\
\end{array}$ * $\begin{array}{|ll|}
{R_1}&{t_1}\\
{0}&{1}\\
\end{array}$  
同理，
