# Reduction from SAT to 3-SAT
---
## 1. SAT and 3-SAT
‌‌‌　　SAT: Given an infinite set of Boolean variables $X=\{x_1,x_2,......,x_n\}$, $|X|=n$,  each variable $x_i$ can take either 0 or 1, a set of clauses $C=\{C_1,C_2,......,C_m\}$, $|C|=m$, $C=C_1\land C_2\land ......\land C_m$, each $C_i$ is a paradigm of multiple variables of descent, which can be expressed as $z_1\lor z_2\lor ......\lor z_k$.
‌‌‌　　Problem: Given a set $X$ of Boolean variables and a set $C$ of clauses, is there an assignment such that $C$ is true, i.e., valid for each $C_i$.
‌‌‌　　3-SAT: A version of the SAT problem in which every clause has 3 literals.
## 2. Process
‌‌‌　　We divide the proof into five steps,which are as follows:

‌‌‌　　1): We’ll prove that 3-SAT is NP problem.
‌‌‌　　The instances of 3-SAT are $X=\{x_1,x_2,......,x_n\}$,  $C=\{C_1,C_2,......,C_m\}$.We verify 3-SAT by assigning values to each literal such that each clause is true. This process is polynomial time.

‌‌‌　　2): CNF reduce to 3-CNF.
‌‌‌　　Given an instance of SAT: $X=\{x_1,x_2,......,x_n\}$,  $C=\{C_1,C_2,......,C_m\}$.We want to convert that instance into an instance of 3-SAT, $X'$,  $C'$.And each $Ci$ has the following cases.
‌‌‌　　Case1: k =1.We need to add the variables $y_{i,1}$, and $y_{i,2}$ to get 
‌‌‌　　$$
‌‌‌　　{C_i}^{'}=\left( z_{i,1}\lor y_{i,1}\lor y_{i,2} \right) \land \left( z_{i,1}\lor \overline{y_{i,1}}\lor \overline{y_{i,2}} \right) \land \left( z_{i,1}\lor y_{i,1}\lor \overline{y_{i,2}} \right) \land \left( z_{i,1}\lor \overline{y_{i,1}}\lor y_{i,2} \right) 
‌‌‌　　$$

‌‌‌　　Case2: k=2.Add a variable $y_{i,1}$ to $C_i$ so that $C_i$ takes the following form.
‌‌‌　　$$
‌‌‌　　{C_i}^{'}=\left( z_{i,1}\lor y_{i,1}\lor z_{i,2} \right) \land \left( z_{i,1}\lor \overline{y_{i,1}}\lor z_{i,2} \right) 
‌‌‌　　$$
‌‌‌　　Case3: k=3.At this point, it is exactly 3-CNF.
‌‌‌　　Case4: k$\geqslant$ 4.Add k variables that will give us$$
‌‌‌　　{C_i}^{'}=\left( z_{i,1}\lor z_{i,2}\lor y_{i,1} \right) \land \left( \overline{y_{i,1}}\lor z_{i,3}\lor y_{i,2} \right) \land ......\land \left( \overline{y_{i,k-3}}\lor z_{i,k-1}\lor z_{i,k} \right) 
‌‌‌　　$$
‌‌‌　　3): Check if the reduce process is polynomial time.

‌‌‌　　4): We can set the literal so that each clause $C_i ^{'}$is true, and thus the whole is true.
‌‌‌　　Case1: k=1.As long as $z_{i,1}=1$ , then we have $C_i ^{'}=1$ regardless of the values of $y_{i,1}, y_{i,2}$.
‌‌‌　　Case2:k=2.When $z_{i,1}\lor z_{i,2}=1$, $y_i$ can take any value.$C_i^{'}=1$.
‌‌‌　　Case3:k=3.$C_i^{'}=1$.
‌‌‌　　Case4:k$\geqslant$ 4.There can be three situations:
‌‌‌　　a): $z_{i,1}\lor z_{i,2}=1$ and $y_{i,j}=0(j=1,2,......,k-3)$.$C_i^{'}=1$.
‌‌‌　　b):$z_{i,k-1}\lor z_{i,k}=1$ and $y_{i,j}=1(j=1,2,...,k-3)$.$C_i^{'}=1$.
‌‌‌　　c):$z_{i,j}=1(2<j<k-1)$ and $\overline{y_{i,a}}=0(a\le j-2)$, $y_{i,a}=0(a\ge j-1)$.$C_i^{'}=1$.

‌‌‌　　From the above, it is clear that SAT can be reduced to 3-SAT.
‌‌‌　　

