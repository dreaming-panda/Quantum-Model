# Quantum Model

We have $W$ qubits for weight and $S$ qubits for our training dataset as well as 1 readout qubit $R$ and 1 qubit represents labels $L$.

then the forward layer $F$ can be viewed as:
$$
|Weight\rangle|Sample\rangle|R\rangle \rightarrow |Weight\rangle|Sample\rangle|f(W,S)\rangle
$$
what we want is that 
$$
f(W,S) = L
$$
we use $CNOT$ to compare the $Readout$ and $Label$
$$
|R\rangle|L\rangle \rightarrow(CNOT) \rightarrow |R \oplus L\rangle|L \rangle
$$
so if $L = R$
$$
|R\rangle|L\rangle \rightarrow(CNOT) \rightarrow |0\rangle|L \rangle
$$
if $L \ne R$
$$
|R\rangle|L\rangle \rightarrow(CNOT) \rightarrow |1\rangle|L \rangle
$$
suppose that
$$
|Sample\rangle | L\rangle = \frac{\sum_{x \in Dataset}|x\rangle|Label(x)\rangle}{\sqrt{\# Dataset}}
$$

$$
|Weight\rangle =\frac{\sum_{0 \le w\le 2^W-1}|w\rangle}{\sqrt{2^W}}
$$
in the initial state
$$
|R\rangle = |0\rangle \\ 
~\ \\ ~\ \\ ~\ \\|Weight\rangle|Sample\rangle|L\rangle|R\rangle = (\frac{\sum_{0 \le w\le 2^W-1}|w\rangle}{\sqrt{2^W}})(\frac{\sum_{x \in Dataset}|x\rangle|Label(x)\rangle}{\sqrt{\# Dataset}})|0\rangle =\\ ~\ \\~\ \\~\ \\
\frac{\sum_{x \in Dataset, 0\le w\le 2^W-1}|w\rangle|x\rangle|Label(x)\rangle |0\rangle}{\sqrt{\#Dataset \times 2^W}} \rightarrow \\ ~\ \\~\ \\~\ \\\frac{\sum_{x \in Dataset, 0\le w\le 2^W-1}|w\rangle|x\rangle|Label(x)\rangle |f(w,x)\rangle}{\sqrt{\#Dataset \times 2^W}} \rightarrow\\ ~\ \\~\ \\~\ \\
\frac{\sum_{x \in Dataset, 0\le w\le 2^W-1}|w\rangle|x\rangle|Label(x)\rangle |f(w,x) \oplus Label(x)\rangle}{\sqrt{\#Dataset \times 2^W}} =\\ ~\ \\~\ \\~\ \\
\sum_{weight}|w\rangle(\sum_{data}|x\rangle|Label(x)\rangle |f(w,x) \oplus Label(x)\rangle) \quad (omit \quad normal \quad factor)\\ ~\ \\~\ \\~\ \\=\sum_{weight}|w\rangle(\sum_{Right}|x\rangle|Label(x)\rangle |0\rangle +\sum_{Wrong}|x\rangle|Label(x)\rangle|1\rangle) (*)
$$
then we $Meassure$  the Last qubit, if the result is $|0\rangle$

The remaining state vector is 
$$
\sum_{weight}|w\rangle(\sum_{Right}|x\rangle|Label(x)\rangle |0\rangle ) \quad(omit \quad normal \quad factor)
$$
then we meassure the first $W$ qubits
$$
p(w) = \frac{\#Right(w)}{\#Right(total)}
$$
so we can get proper weights

 if we introduce another superposition qubit in $(*)$
$$
\frac{\sum_{weight}|w\rangle(\sum_{Right}|x\rangle|Label(x)\rangle |0\rangle +\sum_{Wrong}|x\rangle|Label(x)\rangle|1\rangle)(\frac{1}{\sqrt2}(|0\rangle+|1\rangle))}{\sqrt{\#Dataset  \times 2^W}} = \\ ~\ \\ ~\ \\ 
\frac{1}{\sqrt{\#Dataset  \times 2^{W+1}}}\sum_{weight}|w\rangle(\sum_{Right}|x\rangle|Label(x)\rangle |0\rangle |0\rangle+\sum_{Right}|x\rangle|Label(x)\rangle |0\rangle |1\rangle+\\\sum_{Wrong}|x\rangle|Label(x)\rangle|1\rangle |0\rangle + \sum_{Wrong}|x\rangle|Label(x)\rangle|1\rangle |1\rangle)
$$
Then we apply a CCX gate  to the last 2 qubits and the first $Weight$ qubit

the state vector becomes
$$
\frac{1}{\sqrt{\#Dataset  \times 2^{W+1}}}(\\
(\sum_{weight}|w\rangle\sum_{Right}|x\rangle|Label(x)\rangle |0\rangle |0\rangle) +
\\
(\sum_{weight}|w\rangle\sum_{Right}|x\rangle|Label(x)\rangle |0\rangle |1\rangle) + \\
(\sum_{weight}|w\rangle\sum_{Wrong}|x\rangle|Label(x)\rangle |1\rangle |0\rangle) + \\
(\sum_{weight}|\bar w\rangle\sum_{Wrong}|x\rangle|Label(x)\rangle |1\rangle |1\rangle)\\
)
$$
Then we apply the above process for $W$ times to cover all $W$ weight qubits

the the norm factor becomes
$$
\frac{1}{2^W \times \sqrt{\#Dataset}}
$$
a specific w appears
$$
2^W \times\#Right(w) + \sum_{weight}\#Wrong(weight)
$$
probabilty is
$$
\frac{1}{2^W} \times accuracy + \frac{1}{2^{W+1}}
$$




by this mean, if we repeat the whole above process 

# An Example 

![image-20220727195946577](C:\Users\陈卓明\AppData\Roaming\Typora\typora-user-images\image-20220727195946577.png)

![image-20220727200300444](C:\Users\陈卓明\AppData\Roaming\Typora\typora-user-images\image-20220727200300444.png)

in this circuit, we encode a question that has two features.
$$
Label(00) = 0\\
Label(01) = 0\\
Label(10) = 1 \\
Label(11)= 1
$$
our forward layer is a simple linear function
$$
f(w,x)=w_1x_1+w_2x_2
$$


Not Surprisingly, 
$$
c_2c_1c_0=100
$$
is our solution, which means
$$
w_0 = 0\\
w_1=1
$$
