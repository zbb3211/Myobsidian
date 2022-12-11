### 调整字体大小
```md
\begin{small} 
\begin{equation} 
\ldots你的公式 
\end{equation} 
\end{small}
```
![[Pasted image 20221205155709.png]]
####  公式字体变小，但编号恢复原样
```md
\makeatletter
\renewcommand{\maketag@@@}[1]{\hbox{\m@th\normalsize\normalfont#1}}%
\makeatother
```