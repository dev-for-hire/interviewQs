# Ah, the old *java.util.Map<K,V>* Interface!

### Q. *Name a few concrete implementations of the **java.util.Map<>** interface?*<br>
**Aside**: *When I've been asked this question I've always assumed that they want me to name those which I've used and to be fair I haven't used every single one, so, maybe ask the interviewer for some clarification.<br>Anyways, as of 1.8 here's a few that I've used - notice these are *java.util* types but if you've used any concrete implementations from other frameworks then maybe mention them too and get some extra kudos points!!*<br>
* ConcurrentHashMap
* HashMap
* Hashtable
* TreeMap
* WeakHashMap
* The full list is [here](https://docs.oracle.com/javase/8/docs/api/)
<br><br>
### Q. *Can you explain some differences between HashMap and Hashtable?*<br>
The **HashMap** is **unsynchronized** and also allows for **null** values and **null** key.<br>The Hashtable has also been around much longer - since JDK 1.0, whereas the HashMap is a relative new comer having been introduced in JDK 1.2<br>
if you're looking for a thread-safe, highly-concurrent collection then you should take a look inside *java.util.concurrent* - lots of alternatives to using Hastable for example. 
