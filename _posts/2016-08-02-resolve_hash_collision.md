---
layout: post
title: Resolve hash collision
categories:
- blog
tags:
- Programming
date: 2016-08-02
---


From wiki 

Collision is used in two slightly different senses in theoretical computer science and telecommunications. In computer science it refers to a situation where a function maps two distinct inputs to the same output. In telecommunications, a collision occurs when two nodes of a half duplex transmission mode network attempt to transmit at the same time. 

Basically that is two different key that produced the same hash, so in this case we need to handle it. The example below.

The Michael and Toby after the hash function that mapped to the same hash value, in this case are hash collision. 

![](http://feng.io/static/hash_collision/01.png)



Solutions



1.Re-hash

The idea is when two key has hash collision we hash it again until the collusion has avoided. The disadvantage is inefficient.



2.Linear probing

Lets make this easy to understand, if you want to parking your car and you found the position already taken by someone, then you are looking for the next empty position. 

![](http://feng.io/static/hash_collision/02.jpg)

The below case is John Smith and Sandra Dee has hash collision then we are store the conflicted key to next position.

![](http://feng.io/static/hash_collision/02.png)



```
public class LinearProbingHashST<Key,Value> {
    public LinearProbingHashST(int cap) {
        keys = (Key[]) new Object[cap];
        values = (Value[]) new Object[cap];
        M = cap;
    }

    private int hash(Key key){
        return (key.hashCode() & 0x7fffffff) % M;
    }
    public void put(Key key,Value value){
        //若当前数据含量超过了总容量的一半，则重新调整容量
        if(N>=M/2) resize(2*M);  
        int hashValue = hash(key);
        if (values[hashValue]==null){
            keys[hashValue] = key;
            values[hashValue] = value;
            N++;
        }
        else if(keys[hashValue].equals(key)){
            values[hashValue]=value;
        }
        else{
            while (values[hashValue] != null){
                hashValue = (hashValue+1)%M;
            }
            keys[hashValue] = key;
            values[hashValue] = value;
            N++;
        }
    }
    public Value get(Key key){
        int hashValue = hash(key);
        if (keys[hashValue]!=null&&!keys[hashValue].equals(key)){
            while (keys[hashValue]!=null &&keys[hashValue]!=key){
                hashValue = (hashValue+1)%M;
            }
        }
        return values[hashValue];
    }
}
```

3.Separate chains

![](http://feng.io/static/hash_collision/03.png)

The idea are collisions can be resolved by creating a list of keys that map to the same value .

steps

a) Have a key 

b) Calculate the value of the Key

c) Located the value to position data[hashvalue], hashvalue is a list 

d) If the list is empty then insert it directly 

e) If collision insert to the head of the list


References
http://threezj.com/
