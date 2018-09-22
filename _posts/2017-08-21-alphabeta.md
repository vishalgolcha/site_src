---
layout: post
title: "Alpha Beta Pruning" 
excerpt: "A guide on alpha beta pruning with minimax in adversarial two player games , implementation focused tutorial "
comments: true
---
## Zero sum games

Zero sum games have very special mathematical and computational properties.  They are important for several reasons:  first, they model strictly  adversarial  games
that is games  in  which  the  only  way  for  _P1 [Player 1]_ to  improve  his  payoff  is  to  harm  _P2 [Player 2]_, and  vice  versa .  
The following equation signifies a zero sum game :     
                     <center>
                     $$
                     \begin{align*}
                      u1 + u2 = 0 
                      \end{align*}
                     $$  
                     $$
                     \begin{align*}
                     u2 = -u1 
                      \end{align*}
                     $$
                     </center>  

_P1_  is  trying  to  maximize its own utility _u1_ and  _P2_  is  trying  to  maximize _u2_ . Now, since  _u2  = -u1_ , we can say that  _P2_  is trying to minimize the utility of  _P1_  . To understand the concept of utilities , utility of an action state is higher for _P1_ if it results in _P1_ winning the game and similarly the more negative the utility value is for an action state for _P2_ the better it is .  

## Min-Max algorithm
So how do we go about solving such problems ? An attractive proposition is to build a search tree ! We start with the root node being a initial state of the game _( typically the max node )_,a max node is defined as the node that picks up the maximum utility of all it's children  _( you reach a child node when you perform an action from that initial state )_.  


Now the layer after the root node (max node) contains all nodes which are called as min nodes . And expectedly enough the min nodes pick up the minimum utility of it's children _( relate to this as the second player in the game trying to minimize the opposition's utility )_ . These **min-max**layers alternate until the game is finished _( terminal state : a leaf node )_ and the leaf node is where you assign the utilities to various states of the game . For example if player 1 wins it's assigned a utility of _1_ and if player 2 does then it's assigned a _-1_ , in case of a draw the utility is assigned to _0_  _( assigning the utility is totally  upto you ! )_  .

### Understanding DFS Traversal
We traverse the game tree using a dfs type traversal as mentioned in the gist below . 
{% gist 85d8743fd4ddf7658018312ffd09d27d %}

![initialtree]({{ "/assets/img/init.png" | absolute_url }})Inital Tree Configuration 

{% gist 62faa0e2fc9e93ff214f34fdad76e0da %}

Let's assume a small game tree that finishes in two moves as shown below , the layers alternate as mentioned in minimax algorithm with the final layers consisting of utility values .

![minimax]({{ "/assets/img/mini2.png" | absolute_url }})The state of the tree after applying the minmax algorithm

For a game like this that finishes in 2 moves the minimax is fairly easy to compute , but modelling a game tree when you are playing isolation on a big grid or even a large version of tic tac toe would take quite some time to compute . In complex games the search space would explode exponentially and thus we need to find a way better than this .

## Alpha Beta Pruning 
The full minimax search explores some parts of the tree it doesn't have to.  
The idea is as follows : let's assume you are a greedy greedy node _**( the max node x )**_ but you are also very shrewd in terms of resources you have , you can only visit a few branches of this game tree and not all unlike the minimax algorithm so you evaluate one of it's  children  _y_ , _node y_ returns a utility of _u_ , now you move on to the next kid _z_ , while exploring _node z_ _**( which is a min node )**_ and kids of _z_ , one of the kids of _z_ returns a utility value  $$v < u$$ back  , the question is would you keep exploring other kids of z ?  NO ! , since z is a min node and it will at least return a utility value _v_ , but the greedy _node x_ already has a better solution , hence _**we prune all the remaining kids of z**_ and return to _x_ to explore it's remaining kids , this is the essence of alpha beta pruning .  

Incidentally the children of _x_ know about the maximum utility x has made till they are chosen for exploration , how ?  
x propagates the utility value down to it's children .  

Essential in this pruning process is the presence of a pair $${ ( \alpha , \beta  ) }$$  for each node , inspired from the nature of the two nodes that alternate 
( max , min ) and the two layers communicate with each other when the parent propagates either the $$\alpha$$ or $$\beta$$ depending on nature of the node .  

If i am a min node , i will receive the $$\alpha$$ value from my parent , and keep track of the minimum from my children in the $$\beta$$ value .  

If i am a max node , i will receive the $$\beta$$ value from my parent , and keep track of the maximum from my children in the $$\alpha$$ value .  

Keeping in line with the analogy above if at any point in this recursice sequence $$\alpha$$ > $$\beta$$ for a node , then all it's remaining children must be pruned .

Let's visualize this with the above example :


![alpha-beta]({{ "/assets/img/alpha_init.png" | absolute_url }})*The initial tree  now with $${ ( \alpha , \beta  ) }$$ bounds for each node*


![alpha-beta1]({{ "/assets/img/alpha1.png" | absolute_url }})*Node B updates it's beta , and node A updates it's alpha.*


![alpha-beta1]({{ "/assets/img/alpha2.png" | absolute_url }})*Node C is informed of the max value node A has uptil now , hence alpha propagates down,the beta value for C is not yet -2 as shown [error apologies]*


![alpha-beta1]({{ "/assets/img/alpha3.png" | absolute_url }})*Rest of the kids of node C are pruned as we expected and $$\alpha$$ of node A now propagates to node D*


![alpha-beta1]({{ "/assets/img/alpha5.png" | absolute_url }})*D's $$\beta$$ value is updated after exploring the first child*


![alpha-beta1]({{ "/assets/img/alpha6.png" | absolute_url }})*D's $$\beta$$ value is updated again after encountering a lesser utility value*