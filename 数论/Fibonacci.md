# Fibonacci



求Fibonacci的第n项f<sub>n</sub>：
$$
F_n=\left[ \begin{matrix}f_n&f_{n+1}\end{matrix}\right]*\left[ \begin{matrix}0 &1\\1&1\end{matrix}\right]=\left[ \begin{matrix}f_{n+1}&f_{n+2}\end{matrix}\right]=F_{n+1}\\
$$


求Fibonacci的前n项和S<sub>n</sub>
$$
F_n=\left[ \begin{matrix}f_n&f_{n+1}&S_n\end{matrix}\right]*\left[ \begin{matrix}0 &1&0\\1&1&1\\0&0&1\end{matrix}\right]=\left[ \begin{matrix}f_{n+1}&f_{n+2}&S_{n+1}\end{matrix}\right]=F_{n+1}\\
$$
求Fibonacci的S<sub>n</sub>的前n项和p<sub>n</sub>
$$
F_n=\left[ \begin{matrix}f_n&f_{n+1}&S_n&p_n\end{matrix}\right]*\left[ \begin{matrix}0 &1&0&0\\1&1&1&0\\0&0&1&1\\0&0&0&1\end{matrix}\right]=\left[ \begin{matrix}f_{n+1}&f_{n+2}&S_{n+1}&p_{n+1}\end{matrix}\right]=F_{n+1}\\
$$
