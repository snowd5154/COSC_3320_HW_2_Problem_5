//use case 1:

class Node {
    private String data;
    private int frequency;
    private Node left;
    private Node right;

    public Node(String element, int freq){
        data = element;
        frequency = freq;
        left = null;
        right = null;
    }

    public void setRightChild(Node n)
    {
        right = n;
    }

    public void setLeftChild(Node n){
        left = n;
    }

    public Node getRightChild(){
        return right;
    }

    public Node getLeftChild(){
        return left;
    }

    public String getData(){
        return data;
    }

    public int getFrequency(){
        return frequency;
    }

    public static int getLeftChildIndex(int index) {
        if(((2*index) <= MinHeap.heapSize) && (index >= 1)) {
            return 2*index;
        }
        return -1;
    }

    public static int getRightChildIndex(int index) {
        if((((2*index)+1) <= MinHeap.heapSize) && (index >= 1)) {
            return (2*index)+1;
        }
        return -1;
    }

    public static int getParentIndex(int index){
        if((index > 1 && (index <= MinHeap.heapSize))) {
            return index/2;
        }
        return -1;
    }

    public static void inorder(Node root) {
        if(root != null) {
            inorder(root.getLeftChild());
            System.out.print(" "+root.getFrequency()+" ");
            inorder(root.getRightChild());
        }
    }
}

class MinHeap {
    public static int heapSize = 0;
    public static final int heapArraySize = 100;
    public static final int INF = 100000;

    public static void minHeapify(Node A[], int index) {
        int leftChildIndex = Node.getLeftChildIndex(index);
        int rightChildIndex = Node.getRightChildIndex(index);

        int smallest = index;

        if ((leftChildIndex <= MinHeap.heapSize) && (leftChildIndex>0)) {
            if (A[leftChildIndex].getFrequency() < A[smallest].getFrequency()) {
                smallest = leftChildIndex;
            }
        }

        if ((rightChildIndex <= MinHeap.heapSize) && (rightChildIndex>0)) {
            if (A[rightChildIndex].getFrequency() < A[smallest].getFrequency()) {
                smallest = rightChildIndex;
            }
        }

// smallest is not the node, node is not a heap
        if (smallest != index) {
            Node temp;
            temp = A[index];
            A[index] = A[smallest];
            A[smallest] = temp;
            minHeapify(A, smallest);
        }
    }
}

class MinQueue {

    public static void insert(Node A[], Node a, int key) {
        MinHeap.heapSize++;
        A[MinHeap.heapSize] = a;
        int index = MinHeap.heapSize;
        while((index>1) && (A[Node.getParentIndex(index)].getFrequency() > a.getFrequency())) {
            Node temp;
            temp = A[index];
            A[index] = A[Node.getParentIndex(index)];
            A[Node.getParentIndex(index)] = temp;
            index = Node.getParentIndex(index);
        }
    }

    public static Node[] buildQueue(Node c[], int size) {
        Node[] a = new Node[MinHeap.heapArraySize];
        for(int i=0; i<size; i++) {
            MinQueue.insert(a, c[i], c[i].getFrequency());
        }
        return a;
    }

    public static Node extractMin(Node A[]) {
        Node minm = A[1];
        A[1] = A[MinHeap.heapSize];
        MinHeap.heapSize--;
        MinHeap.minHeapify(A, 1);
        return minm;
    }
}

//use case 2

import java.util.PriorityQueue;
import java.util.Scanner;
import java.util.Comparator;
class HuffmanNode {

    int data;
    char c;

    HuffmanNode left;
    HuffmanNode right;
}

class MyComparator implements Comparator<HuffmanNode> {
    public int compare(HuffmanNode x, HuffmanNode y)
    {

        return x.data - y.data;
    }
}

public class Huffman {
    public static void printCode(HuffmanNode root, String s)
    {

        if (root.left
                == null
                && root.right
                == null
                && Character.isLetter(root.c)) {

            System.out.println(root.c + ":" + s);

            return;
        }

        printCode(root.left, s + "0");
        printCode(root.right, s + "1");
    }
    public static void main(String[] args)
    {

        Scanner s = new Scanner(System.in);
        int n = 6;
        char[] charArray = { 'a', 'b', 'c', 'd', 'e', 'f' };
        int[] charfreq = { 5, 9, 12, 13, 16, 45 };

        PriorityQueue<HuffmanNode> q
                = new PriorityQueue<HuffmanNode>(n, new MyComparator());

        for (int i = 0; i < n; i++) {
            HuffmanNode hn = new HuffmanNode();

            hn.c = charArray[i];
            hn.data = charfreq[i];

            hn.left = null;
            hn.right = null;
            q.add(hn);
        }

        HuffmanNode root = null;
        while (q.size() > 1) {

            HuffmanNode x = q.peek();
            q.poll();

            HuffmanNode y = q.peek();
            q.poll();

            HuffmanNode f = new HuffmanNode();
            f.data = x.data + y.data;
            f.c = '-';

            f.left = x;

            f.right = y;

            root = f;

            q.add(f);
        }

        printCode(root, "");
    }
}
