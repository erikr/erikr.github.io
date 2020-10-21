---
layout: post
title: Elastic net in Scikit-Learn vs. Keras
---

Logistic regression with elastic net regularization is available in `sklearn` and `keras`. To compare these two approaches, we must be able to set the same hyperparameters for both learning algorithms. However, how to do is not immediately obvious.

The objective function and variable names differ between the original paper and these libraries. This blog post describes and reconciles these differences. It is assumed the reader understands the purpose of elastic net and the concepts behind regularization.

---

The elastic net, first proposed by Zou and Hastie ([J. R. Statist. Soc. B. 2005](http://web.stanford.edu/~hastie/Papers/B67.2%20(2005)%20301-320%20Zou%20&%20Hastie.pdf)), adds L1 and L2 penalties of lasso and ridge regression methods to the objective function $ L(\lambda_{1}, \lambda_{2}, \beta) $:

$$
\vert y - X\beta \vert^{2}
+ \lambda_{2} \vert \beta \vert^{2}
+ \lambda_{1} \vert \beta \vert_{1}
$$

The model coefficients $\hat{\beta}$ minimize this objective function:

$$
\underset{\beta}{\operatorname{argmin}} \{L(\lambda_{1}, \lambda_{2}, \beta)\}
$$


Elastic net with $\lambda_{2}=0$ is simply ridge regression. Likewise, elastic net with $\lambda_{1}=0$ is simply lasso.

---

In `sklearn`, per the [documentation for elastic net](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.ElasticNet.html), the objective function $ L $ to minimize is different:

$$
\frac{1}{2n} \| y - Xw \| ^{2}_{2} + \alpha \rho \|w\|_1 + \frac{1}{2} \alpha ( 1 - \rho ) + \|w\|^{2}_{2} \\
$$

Note `l1_ratio` is denoted as $ \rho $ here.

There are several differences between (1) and (3):

1. Model coefficients are denoted by $w$, not $\beta$.
1. Norms are denoted with double instead of single lines.
1. Terms with $L2$ norms are multiplied by $\frac{1}{2}$. This is a mathematical convenience; when the derivative of the term is taken, each of those terms is multiplied by $2$, which cancels the $\frac{1}{2}$. This does not change the solution because 
$ \underset{\beta}{\operatorname{argmin}} ( L ) = \underset{\beta}{\operatorname{argmin}} ( kL ) $ for any real value of $k$.
1. The lasso term (L1 penalty) comes first, whereas in the paper it comes after the ridge regression term (L2 penalty).
1. $\alpha \rho$ is used instead of $\lambda_{1}$
1. $\alpha (1-\rho)$ is used instead of $\lambda_{2}$

The `sklearn` documentation describes the equivalence between the L1 and L2 penalty terms, but use different variable names. To make these equations easier to connect to those in the original Zhou and Hastie paper, I set $a=\lambda_{1}$ and $b=\lambda_{2}$ and use the latter notation below:

$$ \alpha = \lambda_{1} + \lambda_{2} $$

$$ \rho = \frac{\lambda_{1}}{\lambda_{2} + \lambda_{2}} $$

The following Python expressions of the above equations will set elastic net hyperparameters $\alpha$ and $\rho$ for elastic net in `sklearn` using $\lambda_{1}$ and $\lambda_{2}$:


```
alpha = lambda1 + lambda2 
l1_ratio = lambda1 / (lambda1 + lambda2)
```

---

The [`keras` documentation for elastic net](https://keras.io/api/layers/regularizers/#l1l2-function) is minimal. No equation for the objective function is given. The documentation does not even refer to L1 L2 regularization as elastic net. However, the implementation is straightforward; simply use the `l1_l2` regularizer function and set the parameters `l1` and `l2`, which are equivalent to $\lambda_{1}$ and $\lambda_{2}$, respectively):

```
tf.keras.regularizers.l1_l2(l1=0.01, l2=0.01)
```

---

[^1]: 
