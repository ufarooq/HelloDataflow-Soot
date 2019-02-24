# HelloDataflow-Soot
Getting started to write Dataflow analysis in Soot. (Without standard Soot templates)

## Code Guidelines for Soot APIs
1. [See basic steps to access Class and methods from Soot API](https://github.com/ufarooq/HelloTransform-Soot) , following Soot APIs are provided to implement Dataflow Analysis. Assuming, you have a ***SootMethod*** object as ``sootMethod``,  using following snippet get all the Basic Blocks in a method.
```java
Body methodBody = sootMethod.retrieveActiveBody();
BlockGraph blockGraph = new BriefBlockGraph(methodBody);
```
2. (Optional) Create a List object, if you want to re-order basic blocks such as Topological sort or other operations. Since, graph can not be modified directly. 
```java
List<Block> blocks = new ArrayList<Block>(blockGraph.getBlocks());
```
3. Get Basic Blocks and iterate over the instructions. 
```java
Iterator<Block> graphIt = blockGraph.getBlocks().iterator();  
while (graphIt.hasNext()) {  
    Block block = graphIt.next();  
    ...
}
```
4. Use Following API to retrieve all the successor(s) of the basic blocs. (Tip: you will get multiple successors for branch)
```java
List<Block> successors = block.getSuccs();
```
5. Use Following API to get all the predecessor(s) of the basic block. 
```java
List<Block> predecessors = block.getPreds();
```
6. Next, iterate over the Block object and go through the statements. 
```java
Iterator<Unit> blockIt = block.iterator();  
while (blockIt.hasNext()) {  
    Unit unit = blockIt.next();  
    ...  
}
```
7. Soot Provides a special data structure ***FlowSet***,  Which provides basic set operations such as Union between two sets. 
```java
// import soot.toolkits.scalar.FlowSet;
// import soot.toolkits.scalar.ArraySparseSet;

FlowSet<T> firstSet = new ArraySparseSet<T>(); //create set object
// use  firstSet.add(object); to add elements
FlowSet<T> secondSet = new ArraySparseSet<T>(); //create another object and add elements
FlowSet<T> thirdSet = new ArraySparseSet<T>(); //keep this object empty to store results

firstSet.union(secondSet, thirdSet); // writes Union (firstSet+secondSet) to thirdSet
```
8. Similarly, intersection and difference operations can be performed. 
```java
firstSet.difference(secondSet, thirdSet); // writes difference (firstSet-secondSet) to thirdSet
firstSet.intersection(secondSet, thirdSet); // writes Common elements of both sets to thirdSet
``` 
