/**
 * FibonacciHeap
 *
 * An implementation of a Fibonacci Heap over integers.
 */
public class FibonacciHeap
{
    public int size; // number of nodes
    public static int links;
    public static int cuts;
    public int marked;
    public int trees; // number of trees
    public HeapNode min;
    public HeapNode first;


    //*******************
    public HeapNode getFirst(){ return this.first; }

    /**
     * public boolean isEmpty()
     *
     * Returns true if and only if the heap is empty.
     *
     * Time complexity: O(1)
     *
     */
    public boolean isEmpty() {
        return this.size == 0;
    }

    /**
     * public HeapNode insert(int key)
     *
     * Creates a node (of type HeapNode) which contains the given key, and inserts it into the heap.
     * The added key is assumed not to already belong to the heap.
     *
     * Returns the newly created node.
     *
     * Time complexity: O(1)
     */
    public HeapNode insert(int key) {
        HeapNode newnode = new HeapNode(key);
        this.size += 1;
        this.trees += 1;
        if (this.first == null){ // if the heap is empty
            this.first = newnode;
            newnode.next = newnode;
            newnode.prev = newnode;
            this.min = newnode;
            return newnode;
        }
        newnode.next = this.first;
        newnode.prev = this.first.prev;
        this.first.prev.next = newnode;
        this.first.prev = newnode;
        this.first = newnode;
        if (newnode.key < this.min.key){
            this.min = newnode;
        }
        return newnode;
    }

    /**
     * public void deleteMin()
     *
     * Deletes the node containing the minimum key.
     *
     * Time complexity: O(n)
     */
    public void deleteMin() {
        if (this.size == 0){    // empty heap
            return;
        }
        if (this.size == 1){    // single node heap
            this.size = 0;
            this.first = null;
            this.min = null;
            this.trees = 0;
            return;
        }
        if (this.trees == 1){   // single tree heap
            this.deleteroot();
            return;
        }
        if (this.min.child == null){    // minimum has no children
            this.nochilddelete();
            return;
        }
        if (this.min.child != null){    // minimum has children
            this.haschilddelete();
        }
    }

    /**
     * public void deleteroot()
     *
     * Help function to deleteMin
     * Deletes the root of a single tree heap.
     *
     * Time complexity: O(n)
     */
    public void deleteroot(){
        HeapNode delnode = this.min;
        this.first = delnode.child;
        HeapNode helpnode = delnode.child;
        if (helpnode.mark == 1){ // turning the root's child to unmarked
            helpnode.mark = 0;
            this.marked -= 1;
        }
        helpnode = helpnode.next;
        while (helpnode != delnode.child){
            if (helpnode.mark == 1) { //turning all ot the roots sons to unmarked
                helpnode.mark = 0;
                this.marked -= 1;
            }
            helpnode.parent = null;
            helpnode = helpnode.next;
        }
        delnode.child.parent = null;
        delnode.child = null;
        this.size -= 1;
        this.trees = delnode.rank;
        this.findnewmin(); //finding the new minimum of the heap
        this.successiveLinking(); //consolidating all the trees as seen in the lecture
    }

    /**
     * public void nochilddelete()
     *
     * Help function to deleteMin
     * Deletes the root of a single node tree.
     *
     * Time complexity: O(n)
     */
    public void nochilddelete(){
        HeapNode minnode = this.min;
        minnode.prev.next = minnode.next;
        minnode.next.prev = minnode.prev;
        this.size -= 1;
        this.trees -= 1;
        if (this.first == minnode){
            this.first = minnode.next;
        }
        minnode.prev = minnode;
        minnode.next = minnode;
        this.findnewmin(); //finding the new minimum of the heap
        this.successiveLinking(); //consolidating all the trees as seen in the lecture

    }
    /**
     * public void haschilddelete()
     *
     * Help function to deleteMin
     * Deletes the root of a tree with children.
     *
     * Time complexity: O(n)
     */
    public void haschilddelete(){
        HeapNode delnode = this.min;
        HeapNode minnext = delnode.next;
        HeapNode minprev = delnode.prev;
        delnode.child.prev.next = minnext;
        minnext.prev = delnode.child.prev;
        delnode.child.prev = minprev;
        minprev.next = delnode.child;
        this.size -= 1;
        this.trees = this.trees-1+delnode.rank;
        if (this.first == delnode){
            this.first = delnode.child;
        }
        HeapNode helpnode = delnode.child;
        if (helpnode.mark == 1){ //turning the deleted node child to unmarked
            helpnode.mark = 0;
            this.marked -= 1;
        }
        helpnode = helpnode.next;
        while (helpnode != delnode.child){
            if (helpnode.mark == 1){ //turning all the deleted node's child's to unmarked
                helpnode.mark = 0;
                this.marked -= 1;
            }
            helpnode.parent = null;
            helpnode = helpnode.next;
        }
        delnode.child.parent = null;
        delnode.child = null;
        this.findnewmin(); //finding the new minimum of the heap
        this.successiveLinking(); // consolidating all the trees as seen in the lecture
    }
    /**
     * public void findnewmin()
     *
     * Finding the heap's minimum
     *
     * Time complexity: O(n)
     */
    public void findnewmin(){
        this.min = this.first;
        HeapNode root = this.first;
        root = root.next;
        while (root != this.first){ //iterating over the heaps trees to find the minimum of the roots
            if (root.key < this.min.key){
                this.min = root;
            }
            root = root.next;
        }
    }

    /**
     * public HeapNode findMin()
     *
     * Returns the node of the heap whose key is minimal, or null if the heap is empty.
     *
     * Time complexity: O(1)
     */
    public HeapNode findMin() {
        return this.min;
    }

    /**
     * public void meld (FibonacciHeap heap2)
     *
     * Melds heap2 with the current heap.
     *
     * Time complexity: O(1)
     */
    public void meld(FibonacciHeap heap2) {
        if (heap2.size == 0){ //if heap2 is empty do nothing
            return;
        }
        if (this.size == 0){ //if "this" is empty, change all of its fields to heap2 fields
            this.first = heap2.first;
            this.size = heap2.size;
            this.marked = heap2.marked;
            this.min = heap2.min;
            this.trees = heap2.trees;
            return;
        }
        HeapNode heap2first = heap2.first;
        HeapNode heap2last = heap2.first.prev;
        heap2.first.prev.next = this.first;
        heap2.first.prev = this.first.prev;
        this.first.prev.next = heap2first;
        this.first.prev = heap2last;
        this.size += heap2.size;
        this.trees += heap2.trees;
        this.marked += heap2.marked;
        if (heap2.min.key < this.min.key){
            this.min = heap2.min;
        }
    }

    /**
     * public int size()
     *
     * Returns the number of elements in the heap.
     *
     * Time complexity: O(1)
     */
    public int size(){
        return this.size;
    }

    /**
     * public int[] countersRep()
     *
     * Return an array of counters. The i-th entry contains the number of trees of order i in the heap.
     * (Note: The size of the array depends on the maximum order of a tree.)
     *
     * Time complexity: O(n)
     */
    public int[] countersRep() {
        if (this.size == 0){ //return an array of size = 0
            int[] arr = new int[0];
            return arr;
        }
        if (this.size == 1){ //return an array of size = 1
            int[] arr = new int[1];
            arr[0] = 1;
            return arr;
        }
        HeapNode nodes = this.first;
        int b = nodes.rank;
        nodes = nodes.next;
        while (nodes != this.first){ //looking for the biggest rank among the heap's tree's ranks
            if (nodes.rank > b){
                b = nodes.rank;
            }
            nodes = nodes.next;
        }
        int[] arr = new int[b+1]; //initializing the array to the correct size
        HeapNode currnode = this.first;
        arr[currnode.rank] = arr[currnode.rank] + 1;
        currnode = currnode.next;
        while (currnode != this.first){ //updating the array
            int c = currnode.rank;
            arr[c] = arr[c] + 1;
            currnode = currnode.next;
        }
        return arr;
    }

    /**
     * public void delete(HeapNode x)
     *
     * Deletes the node x from the heap.
     * It is assumed that x indeed belongs to the heap.
     *
     * Time complexity: O(n)
     */
    public void delete(HeapNode x) {
        int d = x.key - this.min.key + 1;
        this.decreaseKey(x, d); // decrease-key of the deleted node to min.key-1
        this.deleteMin(); // deleted node is now the min
    }

    /**
     * public void decreaseKey(HeapNode x, int delta)
     *
     * Decreases the key of the node x by a non-negative value delta. The structure of the heap should be updated
     * to reflect this change (for example, the cascading cuts procedure should be applied if needed).
     *
     * Time complexity: O(log(n))
     */
    public void decreaseKey(HeapNode x, int delta) {
        x.key -= delta;
        if (x.parent != null){
            if (x.key <= x.parent.key){
                this.cascadingCut(x, x.parent);
            }
        }
        if (x.key < this.min.key){
               this.min = x;}
    }

    /**
     * public int nonMarked()
     *
     * This function returns the current number of non-marked items in the heap
     *
     * Time complexity: O(1)
     */
    public int nonMarked(){
        int trees = this.trees;
        int marked = this.marked;
        return trees-marked;
    }

    /**
     * public int potential()
     *
     * This function returns the current potential of the heap, which is:
     * Potential = #trees + 2*#marked
     *
     * In words: The potential equals to the number of trees in the heap
     * plus twice the number of marked nodes in the heap.
     *
     * Time complexity: O(1)
     */
    public int potential(){
        int trees = this.trees;
        int marked = this.marked + this.marked;
        return trees+marked;
    }

    /**
     * public static int totalLinks()
     *
     * This static function returns the total number of link operations made during the
     * run-time of the program. A link operation is the operation which gets as input two
     * trees of the same rank, and generates a tree of rank bigger by one, by hanging the
     * tree which has larger value in its root under the other tree.
     *
     * Time complexity: O(1)
     */
    public static int totalLinks() {
        return links;
    }

    /**
     * public static int totalCuts()
     *
     * This static function returns the total number of cut operations made during the
     * run-time of the program. A cut operation is the operation which disconnects a subtree
     * from its parent (during decreaseKey/
     *
     * Time complexity: O(1) 
     */
    public static int totalCuts() {
        return cuts;
    }

    /**
     * public static int[] kMin(FibonacciHeap H, int k)
     *
     * This static function returns the k smallest elements in a Fibonacci heap that contains a single tree.
     * The function should run in O(k*deg(H)). (deg(H) is the degree of the only tree in H.)
     *
     * Time complexity: O(klogH)
     *
     * ###CRITICAL### : you are NOT allowed to change H.
     */
    public static int[] kMin(FibonacciHeap H, int k) {
        if (k == 0){
            int[] arr = new int[0];
            return arr;
        }
        int[] arr = new int[k];
        arr[0] = H.min.key;
        if (k == 1){
            return arr;
        }
        FibonacciHeap helpheap = new FibonacciHeap(); //initializing help heap
        HeapNode Hminchild = H.min.child;
        helpheap.insertasnode(Hminchild.key, Hminchild); //insert the current minimum to the help heap
        Hminchild = Hminchild.next;
        while (Hminchild != H.min.child){ //inserting all the minimum child's to the help heap
            helpheap.insertasnode(Hminchild.key, Hminchild);
            Hminchild = Hminchild.next;
        }
        int i = 1;
        while (i < k){
            HeapNode currmin = helpheap.findMin(); //finding the help heap's minimum
            arr[i] = currmin.key; //adding the minimum to the final array
            helpheap.deleteMin(); //deleting the help heap's minimum
            if (currmin.kminhelpnode.child != null) { //if the deleted node had child's, add them to the help heap
                HeapNode childstoinsert = currmin.kminhelpnode.child;
                helpheap.insertasnode(childstoinsert.key, childstoinsert);
                int keytocheck = childstoinsert.key;
                childstoinsert = childstoinsert.next;
                while (childstoinsert.key != keytocheck ){
                    helpheap.insertasnode(childstoinsert.key, childstoinsert);
                    childstoinsert = childstoinsert.next;
                }
                i += 1;
            }
            else{
                i += 1;
            }
        }
        return arr;
    }
    /**
     * public void insertasnode(int key, HeapNode originalnode)
     *
     * help function for kmin(). insert a node to the helpheap, in a way that saves a pointer to the original node.
     *
     * Time complexity: O(1)
     */
    public void insertasnode(int key, HeapNode originalnode){ // help method to insert for kmin function using extra field
        HeapNode onetoinsert = new HeapNode(key); // creating the new node
        onetoinsert.kminhelpnode = originalnode; // updating the parallel node from the original heap
        if (this.first == null){ // adding to the helpheap in case it's empty
            this.first = onetoinsert;
            onetoinsert.next = onetoinsert;
            onetoinsert.prev = onetoinsert;
            this.min = onetoinsert;
            this.size += 1;
            this.trees += 1;
        }
        else { // adding to the helpheap in case it is not empty
            onetoinsert.next = this.first;
            onetoinsert.prev = this.first.prev;
            this.first.prev.next = onetoinsert;
            this.first.prev = onetoinsert;
            this.first = onetoinsert;
            if (this.min.key > onetoinsert.key) {
                this.min = onetoinsert;
            }
            this.size += 1;
            this.trees += 1;
        }
    }
    /**
     * public HeapNode[] roots()
     *
     * goes over the heap's trees to make an array of the roots.
     *
     * Time complexity: O(n)
     */
    public HeapNode[] roots(){
        HeapNode[] answer = new HeapNode[this.trees];
        HeapNode currnode = this.first;
        answer[0] = currnode;
        currnode = currnode.next;
        int i = 1;
        while (currnode != this.first){ //iterating all over the trees to find the heap's roots
            answer[i] = currnode;
            currnode = currnode.next;
            i += 1;
        }
        return answer;
    }
    /**
     * public HeapNode link(HeapNode x,HeapNode y)
     *
     * link between two ndoes
     *
     * Time complexity: O(1)
     */
    public HeapNode link(HeapNode x,HeapNode y) {
        if (x.rank == 0){ //if both of the trees are from rank = 0
            if (x.key > y.key){
                y.child = x;
                x.parent = y;
                y.rank += 1;
                return y;
            }
            else{
                x.child = y;
                y.parent = x;
                x.rank += 1;
                return x;
            }
        }
        if (x.key > y.key) { //if the trees ranks are different from 0
            return link2(x, y);
        }
        else {
            return link2(y, x);
        }
    }

    /**
     * public HeapNode link2(HeapNode x,HeapNode y)
     *
     * link help function
     * linking two trees
     *
     * Time complexity: O(1)
     */
    public HeapNode link2(HeapNode x, HeapNode y){ //help function to avoid duplicated code
        x.next = y.child;
        y.child.prev.next = x;
        x.prev = y.child.prev;
        y.child.prev = x;
        y.rank += 1;
        y.child = x;
        x.parent = y;
        if (y.mark == 1) {
            y.mark = 0;
        }
        return y;
    }

    /**
     * public void successiveLinking()
     *
     * successivelinking as seen in the class
     *
     * Time complexity: O(n)
     */
    public void successiveLinking() {
        HeapNode[] treesroots = this.roots();
        int size = (int) Math.floor(Math.log(this.size)/Math.log(2));
        HeapNode[] helparray = new HeapNode[2* (size+1)];
        for (HeapNode curr : treesroots) {
            if (helparray[curr.rank] == null) { //checking if the "bucket" is empty
                curr.next = curr;
                curr.prev = curr;
                helparray[curr.rank] = curr;
            }
            else {
                curr.prev = curr;
                curr.next = curr;
                while (helparray[curr.rank] != null) { //keep link between trees until reaching an empty "bucket"
                    curr = this.link(curr, helparray[curr.rank]);
                    links += 1;
                    helparray[curr.rank - 1] = null; //clearing the "bucket"
                }
                helparray[curr.rank] = curr; //adding the tree to the next "bucket"
            }
        }
        this.arrangetree(helparray);
    }

    /**
     * public void arrangetree(HeapNode[] arr)
     *
     * updating the heap after successivelinking happened.
     *
     * Time complexity: O(n)
     */
    public void arrangetree(HeapNode[] arr){ // linking between the new consolidated trees
        HeapNode helpnode = null;
        int numoftrees = 0;
        HeapNode min = null;
        HeapNode first = null;
        for (HeapNode curr : arr){
            if (curr != null){ // updating the first tree
                if (numoftrees == 0){
                    helpnode = curr;
                    first = curr;
                    min = curr;
                    numoftrees += 1;
                }
                else { // linking between the trees
                    helpnode.next = curr;
                    curr.prev = helpnode;
                    curr.next = first;
                    if (curr.key < min.key){
                        min = curr;
                    }
                    first.prev = curr;
                    numoftrees += 1;
                    helpnode = curr;
                }
            }
        }
        this.first = first;
        this.min = min;
        this.trees = numoftrees;
    }

    /**
     * public void cut(HeapNode child ,HeapNode parent)
     *
     * cascadingCut help function
     * cutting a child from its parent
     *
     * Time complexity: O(1)
     */
    public void cut(HeapNode child ,HeapNode parent){
        child.parent = null;
        if (child.mark == 1){ // update marked
            this.marked -= 1;
        }
        child.mark = 0;
        parent.rank -= 1;
        cuts += 1;
        trees += 1;
        if (parent.child == child){
            parent.child = child.next;}
        if (child.next == child){ //  is an only child
            parent.child = null;}
        else{
            child.prev.next = child.next;
            child.next.prev = child.prev;}
        child.prev = this.first.prev; // bypass child, change pointers
        this.first.prev.next = child;
        this.first.prev = child;
        child.next = this.first;
        this.first = child;
    }
    /**
     * public void cascadingCut(HeapNode child ,HeapNode parent)
     *
     * checking for violation between two nodes
     * cutting nodes until no violation
     *
     * Time complexity: O(log(n))
     */
    public void cascadingCut(HeapNode child ,HeapNode parent) {
        cut(child, parent);
        if (parent.parent != null){
            if (parent.mark == 0){ // parent not marked
                parent.mark = 1;
                this.marked += 1;
            }
            else{
                cascadingCut(parent, parent.parent);} // parent is marked
        }
    }


    /**
     * public class HeapNode
     *
     * If you wish to implement classes other than FibonacciHeap
     * (for example HeapNode), do it in this file, not in another file.
     *
     */
    public static class HeapNode{

        public int key;
        public int info;
        public int rank;
        public int mark;
        public HeapNode child;
        public HeapNode next;
        public HeapNode prev;
        public HeapNode parent;
        public HeapNode kminhelpnode;


        public HeapNode(int key) {
            this.key = key;
        }

        /**
         * public int getKey()
         *
         * return nodes key
         *
         * Time complexity: O(1)
         */
        public int getKey() {
            return this.key;
        }

        //***********************
        public int getRank(){ return this.rank; }
        public int getMarked(){ return this.mark; }
        public HeapNode getParent(){ return this.parent; }
        public HeapNode getNext(){ return this.next; }
        public HeapNode getPrev(){ return this.prev; }
        public HeapNode getChild(){ return this.child; }
        public boolean getMarked1() { return this.mark == 1;}
    }

}

