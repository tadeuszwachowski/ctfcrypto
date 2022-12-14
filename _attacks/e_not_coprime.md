---
layout: writeup
title: "e and phi(n) not coprime"
---

## e and phi(n) not coprime

If $e$ and $\varphi(n)$ not coprime - $gcd(e,\varphi(n)) \neq 1$ - then, we must find $d$ in other way.

If 
<div>
$$
n = p_1^{k_1} p_2^{k_2} \dots p_r^{k_r}
$$
</div>
then 
<div>
$$\begin{array}{ll}
	c 	&\equiv m^e \pmod{n} \\
		&\equiv m^e \pmod{p_1^{k_1} p_2^{k_2} \dots p_r^{k_r}} \\
\end{array}$$
</div>
and 
<div>
$$
c \equiv m^e \pmod{p_i^{k_i}}
$$
</div>
because
<div>
$$\begin{array}{ll}
	a \equiv b \pmod{pq} 	&\Rightarrow a-b = k(pq) \\
							&\Rightarrow a-b = (kq)p \wedge a-b = (kp)q \\
							&\Rightarrow a-b \equiv 0 \pmod{p} \wedge a-b \equiv 0 \pmod{q} \\
							&\Rightarrow a \equiv b \pmod{p} \wedge a \equiv b \pmod{q} \\
\end{array}$$
</div>
and then 
<div>
$$\begin{array}{lll} 
\varphi(p) = p-1 &\vee &\varphi(q) = q-1 \\
d \equiv e^{-1} \pmod{\varphi(p)} &\vee &d \equiv e^{-1} \pmod{\varphi(q)} \\
\end{array}$$
</div>

```python
from Crypto.Util.number import long_to_bytes, inverse
from math import gcd

# returns private key - d and n
def non_coprime(e,p,q):
	n = p*q
	phi = (p-1)*(q-1)

	if gcd(e,p-1) == 1:
		n = p
		phi = p-1
	elif gcd(e,q-1) == 1:
		n = q
		phi = q-1
	else:
		print('p-1 and q-1 not coprime with e')
		return
	d = inverse(e,phi)
	return d, n

# change accordingly
e = 18959
p = 8853107629856302430942645802685600792214004993921099904332911487775152756152460899671437787731654521568200225685173143721860070387195312109191089843558621
q = 12263776399134581413994039043220106353464473125114825391625856240762676598269365363349978019785253746863903410731653514543481130557521535535237879154364911
ct = 64521812048352846958817059534278142356568238192123182336017359260377716295619478728140210232152018155950695896362673540987021049139829121799099909484852120051863107269165139203886417085008081775352265576110683356959797391197297615443422020648048621511483229468510937180464189390129089235915976695524813058244

phi = (p-1)*(q-1)
print(gcd(e,phi)) # e and phi are not coprime if not 1

d, n = non_coprime(e,p,q)

print(long_to_bytes(pow(ct,d,n)))
```

