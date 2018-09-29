# Lecture 1: Regression - Case Study

Overview:

A set of function -> Goodness of function $f$ <- Training Data

Step 1: Model

- Linear model: 

  $$y = b + \sum w_i x_i,$$

  where

  - $x_i$: an attribute of input $x$ (feature),
  - $w_i$: weight and
  - $b$: bias.

Step 2: Goodness of Function

Traning Data: $(x^1, \hat y^1)$, $(x^1, \hat y^1)$, $\dots$, $(x^{10}, \hat y^{10})$. This is real data. 

We can get $(x_{cp}^n, \hat y^n)$ based on how many data we have.

To determine *how bad* a function is, we have to declare:

- Loss function $L$: 

  Input: a function, output: how bad it is

  \begin{align}
    L(f) & = L(w, b) \\\\
         & = \sum_{n = 1}^{10} (\hat y^n - (b + w \cdot x_{cp}^n))^2.
  \end{align}

We have to pick the "best" function

\begin{align}
f^\* & = arg \min_f L(f) \\\\
w^\*, b^\* & = arg \min_{w, b} L(w, b) \\\\
         & = arg \min_{w, b} \sum_{n = 1}^{10} (\hat y^n - (b + w \cdot x_{cp}^n))^2.
\end{align}

Step 3: Gradient Descent

Consider loss function $L(w)$ with one parameter $w$:

- (Randomly) Pick an initial value $w^0$
- Compute $\frac{dL}{dw}\Big |_{w = w^0}$
    - if $\frac{dL}{dw}\Big |_{w = w^0} < 0$: increase $w$
    - if $\frac{dL}{dw}\Big |_{w = w^0} > 0$: decrease $w$

  $$w^1 \leftarrow w^0 - \eta \frac{dL}{dw}\Big |_{w = w^0}$$ 
  
  where $\eta$: learning rate.

- Compute $\frac{dL}{dw}\Big |_{w = w^1}$

  $$w^2 \leftarrow w^1 - \eta \frac{dL}{dw}\Big |_{w = w^1}$$

$$\vdots$$

Consider two parameters $w$, $b$:

- (Randomly) Pick initial value $w^0$, $b^0$
- Compute $\frac{\partial L}{\partial w}\Big |\_{w = w^0, b = b^0}$ and $\frac{\partial L}{\partial b}\Big |\_{w = w^0, b = b^0}$

  $$w^1 \leftarrow w^0 - \eta \frac{\partial L}{\partial w}\Big |\_{w = w^0, b = b^0} \qquad b^1 \leftarrow b^0 - \eta \frac{\partial L}{\partial b}\Big |\_{w = w^0, b = b^0}$$