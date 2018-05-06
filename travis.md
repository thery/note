# Adding travis for a Coq library under github

What has been done to get [travis](https://travis-ci.org/)
checking the Coq libray  [Coqprime](https://github.com/thery/coqprime) 
starting from the 
[opam installer](https:
Start from the [opam installer](https://github.com/thery/coqprime/blob/master/opam/opam)
//github.com/thery/coqprime/blob/master/opam/opam)


##1. Sign in into [travis](https://travis-ci.org/) 

##2. Enable [Coqprime](https://github.com/thery/coqprime) 

![Enable](./travis_coqprime.png)

##3. Create a [.travis.yml](https://github.com/thery/coqprime/blob/master/.travis.yml) file.

##4. Set up a cache:

````
cache:
  apt: true
  directories:
  - $HOME/.opam
````

##5. Make opam available

````
addons:
  apt:
    sources:
    - avsm
    packages:
    - opam
    - aspcud
    - gcc-multilib
````

##6. Translate opam dependency

```
depends: [
  "coq" {>= "8.8~"}
  "coq-bignums"
]
```

into travis installation

````
install:
- export OPAMROOT=$HOME/.opam
- opam init -j ${NJOBS}  -n -y
- eval $(opam config env)
- opam repo add coq-released http://coq.inria.fr/opam/released || echo "coq-released registered"
- opam install coq.${COQ_VER} -y
- opam install coq-bignums -y
````


##7. Translate opam build

````
build: [
  ["./configure.sh"]
  ["make" "-j%{jobs}%"]
  ["make" "install"]
]
````

into travis script


````
script:
 - ./configure.sh
 - make -j ${NJOBS}
 - make install


````

