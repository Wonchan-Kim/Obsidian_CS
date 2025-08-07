# Ford Fulkerson Algorithm
```psuedocode
FF(G, c, s, t) // c is capacity
Let f be a 0 flow, f(e) = 0 for all edges
While there is path in the residual network(Gf, cf, s, t):
	find a path P in the residual graph Gf from s to t
	Augment the flow along every edge in P, updating the flow f

Augment(f, P):
let c min = min cf(e) - the bottleneck capacity for P
for every forward edge e of P, update f(e) = f(e) + cmin
for every reverse edge e of P, update f(eR) = f(eR) - cmin
```
At most W|V| iterations of outside loop of FF, each iteration takes O(|E|) times, so overall time O(|VE|) where W is the max capacity of an edge in G. 

Meaning, if W is large there is the possibility we spend more time per path search, tying to find the better path.