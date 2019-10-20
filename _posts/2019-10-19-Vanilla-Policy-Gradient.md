---
title: "Vanilla policy gradient"
categories:
  - papers
tags:
  - reinforcement learning
---

 The value-function approach on reinforcement learning has worked well in many applications. It obtains the policy by selecting the action in each state with highest estimated value iteratively.

 But the approach faces several probelms. First, the policy obtained is a deterministic policy whereas the optimal policy is often stochastic in reality. Second, a little change to value fuction may result in a diffrent action is selected, which becomes a key obstacle of convergence assurances for algorithms following the value-function apporoach like Q-learning, Sarsa and so on.  

 Sutton,David, et al explore an alternative approach in which the policy is explicitly represented by its own function approximator, independent of the value function, and is updated according to the gradient of expected reward with respect to the policy parameters.  

In reinforcement learning, the state, action, and reward are denoted \\(s_t \epsilon \mathit{S}\\),\\(a_t \epsilon \mathit{A}\\) and 
\\(r_t \epsilon \mathit{R}\\) respectively. The environment's dynamics are charactrized by state transition probabilities, \\( P_{ss'}^{a}=P_r\left \\{ s_{t+1}=s'|s_t=s,a_t=a\right \\} \\) and expected rewards 
\\( R_{s}^{a}=E\left \\{r_{t+1}|s_t=s,a_t=a\right \\} \\), \\( \forall s,s' \epsilon S, a \epsilon A  \\). The agent makes decision by a policy, \\( \pi(s,a,\theta )=P_r(a_t=a|s_t=s,\theta) \\), \\( \forall s \epsilon S, a \epsilon A  \\), where \\( \theta \epsilon \Re^l \\) is a paramter vector.

There are two ways to formulate the agent's objective with function approximation. The first is the average reward formulation. The long-term expected rewards per step, \\( \rho(\pi) \\) is the objective:  

$$ \rho(\pi)= \lim_{n \to\infty  }\frac{1}{n}E\left\{r_1+r_2+....+r_n|\pi\right \}=\sum_{s}d^{\pi}(s)\sum_{a}\pi(s,a)R_s^a, $$

where \\( d^{\pi}(s)=\lim_{t \to\infty}=P_r \left \\{s_t=s_0, \pi \right \\} \\) which is the stationary distribution of states under \\( \pi \\).  
The state-action value function is defined as:

$$ Q^{\pi}(s,a)=\sum_{t=0}^{\infty}E\left \{ r_t- \rho(\pi)|s_0=s, a_0=a, \pi \right \},\forall s \epsilon S, a \epsilon A. $$

The second formulation is that in which there is a designated start state \\( s_0 \\). The objective \\( \rho(\pi) \\) is defined as:

$$ \rho(\pi)=E\left \{ \sum_{t=1}^{\infty}r^{t-1}r_t|s_0, \pi \right\},  $$

And the state-action value function is defined as:

$$ Q^{\pi}(s,a)=E\left\{ \sum_{k=1}^{\infty}r^{k-1}r_t+k|s_t=s,a_t=a,\pi\right \}, $$

where \\( r \epsilon \left \[0,1 \right\] \\) is a discount rate.  

 Let \\( \theta \\) denote the vector of policy\\(\pi(s,a,\theta)\\) parameters and \\( \rho(\pi) \\) the performance of the corresponding policy, the policy gradient is:

 $$ \frac{\partial \rho}{\partial \theta}=\sum_{s}d^{\pi}(s)\sum_{a}\frac{\partial \pi(s,a)}{\partial \theta} Q^{\pi}(s,a)  \quad \quad  (1), $$
 
 where the \\(d^{\pi}(s) \\) has no dependence on \\( \theta \\).  
 In equation (1), \\( Q_{\pi}(s,a) \\) is not konwn and must be estimated. Of course, a natural thought is to use the actual returns, \\( R_t=\sum_{k=1}^{\infty}r_{t+k}-\rho(\pi)\\).  
 Now consider if \\(Q^{\pi} \\) can be approximated by a learned function.   
 Assumate \\( f_w:S\times A\rightarrow \Re \\) is the approximation to \\( Q^{\pi} \\) with paramter \\(w\\).  It is natural to learn \\( f_w \\)  by following \\( \pi \\) and updating \\(w\\) by minize:

 $$ \left [ \hat{Q_{\pi}}(s_t,a_t) - f_w(s_t, a_t) \right ]^{2},  $$
 
 where \\( \hat{Q_{\pi}}(s_t, a_t) \\) is the unbiased estimator of \\( Q_{\pi}(s_t, a_t)\\).  
 When such a process has converged to a local optimum, then:

 $$ \sum_{s}d^{\pi}(s)\sum_{a}\pi(s,a)\left [ Q_{\pi}(s_t,a_t) - f_w(s_t, a_t) \right ]\frac{\partial f_w(s,a)}{\partial w}=0. \quad \quad (2) $$
 
 if\\(f_w\\) is compatible with the policy parameterization that
 
$$ \frac{\partial f_w(s,a)}{\partial w}=\frac{\pi(s,a)}{\partial \theta} \frac{1}{\pi(s,a)}. \quad \quad (3) $$

Combine (2) and (3) then

 $$ \frac{\partial \rho}{\partial \theta}=\sum_{s}d^{\pi}(s)\sum_{a}\frac{\partial \pi(s,a)}{\partial \theta} f_w(s,a).\quad\quad(4)$$

 The detail how to get (4) and how to proof the convergence of the policy iteration by equation (4), please see the [paper (1)](#reference_1) in the reference.

## Reference
(1)<a id='reference_1'> </a>[Sutton, Richard S., et al. "Policy gradient methods for reinforcement learning with function approximation." Advances in neural information processing systems. 2000.](http://papers.nips.cc/paper/1713-policy-gradient-methods-for-reinforcement-learning-with-function-approximation.pdf)





 
 

