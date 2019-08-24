# Baby Jubjub elliptic curve

Baby Jubjub is a twisted Edwards elliptic curve that was deterministically created over the prime finite field of order 

```p = 21888242871839275222246405745257275088548364400416034343698204186575808495617```,

where the prime above is the order of the BN128 elliptic curve. 

This repository contains the algorithm used to find the curve and 
supporting evidence that the Baby Jubjub elliptic curve, based upon ["Jubjub"](https://z.cash/technology/jubjub.html), satisfies the [SafeCurves criteria](https://safecurves.cr.yp.to/index.html).

1. You can fastly generate the curve by running `sage findCurve.sage 168698`.
This is the lowest ``A=168698`` of a Montgomery curve that statifies the 
critieria defined in [ref7748](https://tools.ietf.org/html/rfc7748).

2. The script ``verify.sage`` is based on
[this script from the SafeCurves site](https://safecurves.cr.yp.to/verify.html),
modified by [Daira Hopwood](https://github.com/daira) to:

    * support twisted Edwards curves,
    * generate a file 'primes' containing the primes needed for primality proofs (if it is not already present),
    * change the directory in which Pocklington proof files are generated
    (``proof/`` rather than ``../../../proof``) and to create that directory if it does not exist.
    
    Prerequisites:

    * apt-get install sagemath
    * pip install sortedcontainers

    Run ``sage verify.sage .``, or ``./run.sh`` to also print out the results.
    
    Note that the "rigidity" criterion cannot be checked automatically.

# Characteristics of the curve

Baby Jubjub is a twisted Edwards curve defined over the finite field of prime order 

```p = 21888242871839275222246405745257275088548364400416034343698204186575808495617```.

The curve has order

``n = 21888242871839275222246405745257275088614511777268538073601725287587578984328``,

which factors in ``n = h x l``, where ``h = 8`` (called *cofactor*) and

`` l = 2736030358979909402780800718157159386076813972158567259200215660948447373041``.

# Forms of the curve

**Montgomery form**

- Equation: ``By^2 = x^3 + A x^2 + x``
- Parameters: ``A = 168698, B = 1``
- Generator point: 

    ``(7, 4258727773875940690362607550498304598101071202821725296872974770776423442226)``
- Base point:

    ``(7117928050407583618111176421555214756675765419608405867398403713213306743542, 14577268218881899420966779687690205425227431577728659819975198491127179315626)``

**Twisted Edwards form**

- Equation: ``ax^2 + y^2 = 1 + dx^2y^2``
- Parameters: ``a = 168700, d = 168696``
- Generator point:  

    ``(995203441582195749578291179787384436505546430278305826713579947235728471134, 5472060717959818805561601436314318772137091100104008585924551046643952123905)``

- Base point:

    ``(5299619240641551281634865583518297030282874472190772894086521144482721001553, 16950150798460657717958625567821834550301663161624707787222815936182638968203)``

**Reduced twisted Edwards form** (optimal)

- Equation: ``a' x^2 + y^2 = 1 + d' x^2y^2``
- Parameters: ``a' = -1``, 
  ``d' = 12181644023421730124874158521699555681764249180949974110617291017600649128846``
- Generator point: 

    ``(4986949742063700372957640167352107234059678269330781000560194578601267663727, 5472060717959818805561601436314318772137091100104008585924551046643952123905)``

- Base point:

    ``(9671717474070082183213120605117400219616337014328744928644933853176787189663, 16950150798460657717958625567821834550301663161624707787222815936182638968203)``

# Conversion of points
Following formulas allow to convert points from one form of the curve to another. We will denote the coordinates
* ``(u, v)`` for points in the Montomgery form, 
* ``(x, y)`` for points in the Twisted Edwards form and 
* ``(x', y')`` for points in reduced Twisted Edwards form.

In last conversions, you will need the scaling factor ``f`` which is

``f = 6360561867910373094066688120553762416144456282423235903351243436111059670888``.

You can also use  

``-f = 15527681003928902128179717624703512672403908117992798440346960750464748824729``.

**Montgomery --> Twisted Edwards**

Change of coordinates ``(u, v) --> (x, y)`` via the map ```x = u/v, y = (u-1)/(u+1)```.

**Twisted Edwards --> Montgomery**

Change of coordinates ``(x, y) --> (u, v)`` via the map  ```u = (1+y)/(1-y), v = (1+y)/((1-y)x)```.

**Montgomery --> Reduced Twisted Edwards**

Change of coordinates ``(u, v) --> (x', y')`` via the map  ```x' = u*(-f)/v, y' = (u-1)/(u+1) ```.

**Reduced Twisted Edwards --> Montgomery**

Change of coordinates ``(x', y') --> (u, v)`` via the map  ```u = (1+y')/(1-y'), v = ((-f)*(1+y')/((1-y')*x')```.

**Twisted Edwards --> Reduced Twisted Edwards**

Change of coordinates ``(x, y) --> (x', y')`` via the map  ```x' = x*(-f), y' = y```.

**Reduced Twisted Edwards --> Twisted Edwards**

Change of coordinates ``(x', y') --> (x, y)`` via the map  ```x = x'/(-f), y = y'```.
