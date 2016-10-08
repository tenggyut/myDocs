##Visual Information Theory

- Conditional Probability: 条件概率

- Marginal Probability: 边缘概率, opposite of Conditional Prob, probability without considering other variable

- Simpson’s Paradox: kidney stone treatment A and B, on overall survival rate, B beats A。 But A beats B if kidney stone are large or small.

- Entropy of distribution: the least average message length

- prefix code: code should not be other codes prefixa

- entropy equation:
$$
H(p) = \sum\limits_{x}p(x){\log_2 (\frac{1}{p(x)})}
$$

- Cross-Entropy:
$$
H_p(q) = \sum\limits_{x}q(x){\log_2 (\frac{1}{p(x)})}
$$

- KL divergence: Prob distribution *P* 
$$
D_q(p) = H_q(p) - H(p)
$$

- Mutual Information:
$$
I(X,Y) = H(X) + H(Y) - H(X,Y)
$$
$
2I(X,Y) = 2H(X) + 2H(Y) - 2H(X,Y) 
= 2H(X) + 2H(Y) - (H(X) + H(X|Y)) - (H(Y) + H(Y|X))
= 2I(X,Y)
$

- Variation Information:
$
V(X,Y) = H(X,Y) - I(X,Y)
$